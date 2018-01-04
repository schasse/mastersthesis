# Test Measures
## DIN/apache
### deploy
| 1. execution | deploy (6)           | 2017-12-29 10:45:47 |
| 1. execution | apply to cluster (9) | 2017-12-29 10:45:58 | 11
| 1. execution | running in cluster   | 2017-12-29 10:45:59 | 1

| 2. execution | deploy (6)           | 2017-12-29 10:46:30 |
| 2. execution | apply to cluster (9) | 2017-12-29 10:46:38 | 8
| 2. execution | running in cluster   | 2017-12-29 10:46:39 | 1

| 3. execution | deploy (6)           | 2017-12-29 10:47:02 |
| 3. execution | apply to cluster (9) | 2017-12-29 10:47:11 | 9
| 3. execution | running in cluster   | 2017-12-29 10:47:12 | 1

### rollback
| 1. execution | running canary (11)   | 2018-01-04 15:42:20 |
| 1. execution | trigger rollback (12) | 2018-01-04 15:44:29 | 129
| 1. execution | apply to cluster (9)  | 2018-01-04 15:44:33 | 4
| 1. execution | running in cluster    | 2018-01-04 15:44:34 | 1

| 2. execution | running canary (11)   | 2018-01-04 16:07:19 |
| 2. execution | trigger rollback (12) | 2018-01-04 16:10:24 | 185
| 2. execution | apply to cluster (9)  | 2018-01-04 16:10:27 | 3
| 2. execution | running in cluster    | 2018-01-04 16:10:28 | 1

| 3. execution | running canary (11)   | 2018-01-04 16:12:59 |
| 3. execution | trigger rollback (12) | 2018-01-04 16:19:39 | 400
| 3. execution | apply to cluster (9)  | 2018-01-04 16:19:42 | 3
| 3. execution | running in cluster    | 2018-01-04 16:19:43 | 1


## Gapfish/rails app
### deploy
| 1. execution | new version push (1)      | 2017-12-29 15:12:37 | 4
| 1. execution | build (3)                 | 2017-12-29 15:12:41 | 348
| 1. execution | save container images (4) | 2017-12-29 15:18:29 | 90
| 1. execution | deploy (6)                | 2017-12-29 15:19:59 | 38
| 1. execution | apply to cluster (9)      | 2017-12-29 15:20:37 | 106
| 1. execution | running in cluster        | 2017-12-29 15:22:23 |

| 2. execution | new version push (1)      | 2017-12-29 15:23:13 | 7
| 2. execution | build (3)                 | 2017-12-29 15:23:20 | 131
| 2. execution | save container images (4) | 2017-12-29 15:25:31 | 51
| 2. execution | deploy (6)                | 2017-12-29 15:26:22 | 22
| 2. execution | apply to cluster (9)      | 2017-12-29 15:26:44 | 67
| 2. execution | running in cluster        | 2017-12-29 15:27:51 |

| 3. execution | new version push (1)      | 2017-12-29 15:28:25 | 4
| 3. execution | build (3)                 | 2017-12-29 15:28:29 | 145
| 3. execution | save container images (4) | 2017-12-29 15:30:54 | 26
| 3. execution | deploy (6)                | 2017-12-29 15:31:20 | 69
| 3. execution | apply to cluster (9)      | 2017-12-29 15:32:29 | 35
| 3. execution | running in cluster        | 2017-12-29 15:33:04 |

## bad influeces:
* image build time
* pod termination time

## does not matter:
* pod count (rolling update can be made in bigger batches)
* repo size does not matter (cached)
* image size (cached)

# monitors

## 200 diffs
### raw
```
abs( 1 - ( avg:stage.apache.GET.200.count{*}.as_rate().rollup(sum, 30) / ( 2 * avg:stage.apache.canary.GET.200.count{*}.as_rate().rollup(sum, 30) ) ) )
```
### better readable
```
ratio = apache_GET_200_count.rollup(sum, 30) / (2 * apache_canary_GET_200.rollup(sum, 30) )
threshold = 3
abs(1 - ratio) > 3
```


## redirect diffs
### raw
```
abs( 1 - ( ( avg:stage.apache.GET.301.count{*}.as_rate().rollup(sum, 360) + avg:stage.apache.GET.302.count{*}.as_rate().rollup(sum, 360) ) / ( 2 * ( avg:stage.apache.canary.GET.301.count{*}.as_rate().rollup(sum, 360) + avg:stage.apache.canary.GET.302.count{*}.as_rate().rollup(sum, 360) ) ) ) )
```
### better readable
```
permanent_redirects = apache_GET_301_count.rollup(sum, 360) + apache_GET_302_count.rollup(sum, 360)
temporary_redirects = apache_canary_GET_301_count.rollup(sum, 360) + apache_canary_GET_302_count.rollup(sum, 360)
ratio = permanent_redirects / (2 * temporary_redirects)
threshold = 1.5
abs(1 - ratio) > threshold
```
