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
                ( ($panel.title | ascii_downcase) | startswith(($targetTitle | ascii_downcase)))
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
    | .templating.list = (.templating.list | map(select((.name | ascii_downcase) != ($targetTitle | ascii_downcase))))
'

curl -fsSL https://raw.githubusercontent.com/thecfu/scraparr/refs/tags/v2.1.2/dashboards/dashboard1.json \
    | sed 's/${DS_MIMIR}/prometheus/g' \
    | jq '.title = "Scraparr"' \
    | jq 'with_entries(select(.key | startswith("__") | not))' \
    | jq --arg targetTitle "Seerr" "$jq_filter" \
    | jq --arg targetTitle "Readarr" "$jq_filter" \
    > grafana-dashboard.json
```
