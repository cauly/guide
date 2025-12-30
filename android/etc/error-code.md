# Error Code

Cauly SDK에서 정의하는 Error Cod는 다음과 같습니다.

| Code | Message                                                                     | 설명                              |
| ---- | --------------------------------------------------------------------------- | ------------------------------- |
| 0    | OK                                                                          | 유료 광고                           |
| 200  | No filled AD                                                                | 전면CPM 광고 없음                     |
| 400  | The app code is invalid. Please check your app code!                        | App code 불일치 또는default app code |
| 500  | Server error                                                                | cauly서버 에러                      |
| -100 | SDK error                                                                   | SDK 에러                          |
| -200 | Request Failed(You are not allowed to send requests under minimum interval) | 최소요청주기 미달                       |
