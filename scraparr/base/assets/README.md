# Grafana Dashboard

```bash
jq_filter='
    .panels as $panels
    | .panels = (
        $panels
        | reduce range(0; length) as $i (
            {skip: false, result: []};
            . as $state
            | $panels[$i] as $panel
            | if (
                $panel.type == "row" and
                ( ($panel.title | ascii_downcase) | startswith(($title | ascii_downcase)))
            ) then
                $state + {skip: true}
            elif ($panel.type == "row" and $state.skip) then
                $state + {skip: false}
            else
                $state
            end
            | if .skip then . else .result += [$panel] end
        ) | .result
    )
    | .templating.list = (.templating.list | map(select((.name | ascii_downcase) != ($title | ascii_downcase))))
'

jq_sort='
  # Group panels into sections by row
  .panels as $original_panels |
  (reduce range(0; $original_panels | length) as $i (
    {"current": null, "sections": {}, "result": []};
    
    if $original_panels[$i].type == "row" then
      # Start a new section when we hit a row
      .current = ($original_panels[$i].title | split(" ")[0]) |
      .sections[$original_panels[$i].title | split(" ")[0]] = [.result | length] |
      .result += [$original_panels[$i]]
    elif .current != null then
      # Add panel to current section
      .sections[.current] += [.result | length] |
      .result += [$original_panels[$i]]
    else
      # Handle panels that come before any row
      .result += [$original_panels[$i]]
    end
  )) as $grouped |
  
  # Only update the panels field while preserving everything else
  . + {
    "panels": (
      reduce $titles[] as $section (
        []; 
        . + (
          if $grouped.sections[$section] then
            # Get all panels in this section
            [($grouped.sections[$section][0] | tonumber)] + 
            $grouped.sections[$section][1:] | 
            map($grouped.result[.])
          else
            []
          end
        )
      )
    )
  }
'

# Process the dashboard JSON
curl -fsSL "https://raw.githubusercontent.com/thecfu/scraparr/refs/tags/v2.1.2/dashboards/dashboard1.json" \
  | sed 's/${DS_MIMIR}/prometheus/g' \
  | jq '.title = "Scraparr"' \
  | jq 'with_entries(select(.key | startswith("__") | not))' \
  | jq --arg title "Seerr" "$jq_filter" \
  | jq --arg title "Readarr" "$jq_filter" \
  | jq --argjson titles '["Sonarr", "Radarr", "Bazarr", "Prowlarr"]' "$jq_sort" \
  > grafana-dashboard.json
```
