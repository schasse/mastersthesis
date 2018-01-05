# Test Measures
## Speed

### rollback (gapfish)
| trigger rollback (12) | 2017-12-13 16:16:51 | 24
| apply to cluster (9)  | 2017-12-13 16:17:15 | 70
| running in cluster    | 2017-12-13 16:18:25 |

| trigger rollback (12) | 2017-12-13 16:16:51 | 24
| apply to cluster (9)  | 2017-12-13 16:17:15 | 70
| running in cluster    | 2017-12-13 16:18:25 |

| trigger rollback (12) | 2017-12-13 16:16:51 | 24
| apply to cluster (9)  | 2017-12-13 16:17:15 | 70
| running in cluster    | 2017-12-13 16:18:25 |

| nr | step                        |
|----+-----------------------------|
|  1 | push new version            |
|  2 | ci pulls code               |
|  3 | build                       |
|  4 | save container image(s)     |
|  5 | run tests                   |
|  6 | deploy                      |
|  7 | pull infrastructure code    |
|  8 | modify resource definitions |
|  9 | apply to production cluster |
| 10 | pull container image(s)     |
| 11 | send metrics                |
| 12 | trigger rollback            |


deploy(6)-pull infrastructure code(7)-modify resource definitions(8)


| plot       | exec |    steps | timestamp           | seconds |
|------------+------+----------+---------------------+---------|
|------------+------+----------+---------------------+---------|
| deploy     |   1. |      6-8 | 2017-12-29 10:45:47 |      11 |
| din        |      |        9 | 2017-12-29 10:45:58 |       1 |
|            |      | runnning | 2017-12-29 10:45:59 |         |
|            |   2. |      6-8 | 2017-12-29 10:46:30 |       8 |
|            |      |        9 | 2017-12-29 10:46:38 |       1 |
|            |      | runnning | 2017-12-29 10:46:39 |         |
|            |   3. |      6-8 | 2017-12-29 10:47:02 |       9 |
|            |      |        9 | 2017-12-29 10:47:11 |       1 |
|            |      | runnning | 2017-12-29 10:47:12 |         |
|------------+------+----------+---------------------+---------|
| deploy     |   1. |      6-8 | 2017-12-29 15:19:59 |      38 |
| gapfish    |      |        9 | 2017-12-29 15:20:37 |     106 |
|            |      | runnning | 2017-12-29 15:22:23 |         |
|            |   2. |      6-8 | 2017-12-29 15:26:22 |      22 |
|            |      |        9 | 2017-12-29 15:26:44 |      67 |
|            |      | runnning | 2017-12-29 15:27:51 |         |
|            |   3. |      6-8 | 2017-12-29 15:31:20 |      30 |
|            |      |        9 | 2017-12-29 15:31:50 |      70 |
|            |      | runnning | 2017-12-29 15:33:00 |         |
|------------+------+----------+---------------------+---------|
| rollback   |   1. |   12,7-8 | 2018-01-04 15:44:29 |       4 |
| din        |      |        9 | 2018-01-04 15:44:33 |       1 |
|            |      | runnning | 2018-01-04 15:44:34 |         |
|            |   2. |   12,7-8 | 2018-01-04 16:10:24 |       3 |
|            |      |        9 | 2018-01-04 16:10:27 |       1 |
|            |      | runnning | 2018-01-04 16:10:28 |         |
|            |   3. |   12,7-8 | 2018-01-04 16:19:39 |       3 |
|            |      |        9 | 2018-01-04 16:19:42 |       1 |
|            |      | runnning | 2018-01-04 16:19:43 |         |
|------------+------+----------+---------------------+---------|
| metric     |   1. |       11 | 2018-01-04 15:42:20 |     129 |
| comparison |      |       12 | 2018-01-04 15:44:29 |         |
| din        |   2. |       11 | 2018-01-04 16:07:19 |     185 |
|            |      |       12 | 2018-01-04 16:10:24 |         |
|            |   3. |       11 | 2018-01-04 16:12:59 |     400 |
|            |      |       12 | 2018-01-04 16:19:39 |         |
|------------+------+----------+---------------------+---------|
| whole      |   1. |      1-2 | 2017-12-29 15:12:37 |       4 |
| pipeline   |      |        3 | 2017-12-29 15:12:41 |     348 |
| gapfish    |      |        4 | 2017-12-29 15:18:29 |      90 |
| (step 5    |      |      6-8 | 2017-12-29 15:19:59 |      38 |
| skipped)   |      |        9 | 2017-12-29 15:20:37 |     106 |
|            |      | runnning | 2017-12-29 15:22:23 |         |
|            |   2. |      1-2 | 2017-12-29 15:23:13 |       7 |
|            |      |        3 | 2017-12-29 15:23:20 |     131 |
|            |      |        4 | 2017-12-29 15:25:31 |      51 |
|            |      |      6-8 | 2017-12-29 15:26:22 |      22 |
|            |      |        9 | 2017-12-29 15:26:44 |      67 |
|            |      | runnning | 2017-12-29 15:27:51 |         |
|            |   3. |      1-2 | 2017-12-29 15:28:25 |       4 |
|            |      |        3 | 2017-12-29 15:28:29 |     145 |
|            |      |        4 | 2017-12-29 15:30:54 |      26 |
|            |      |      6-8 | 2017-12-29 15:31:20 |      69 |
|            |      |        9 | 2017-12-29 15:32:29 |      35 |
|            |      | runnning | 2017-12-29 15:33:04 |         |

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
