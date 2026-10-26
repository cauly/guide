# Error Code

| Code | Message                                                                     | 설명                                                    |
| ---- | --------------------------------------------------------------------------- | ----------------------------------------------------- |
| 0    | OK                                                                          | 유료 광고                                                 |
| 100  | Non-chargeable ad is supplied                                               | 무료 광고(속성 : 공익 광고, Cauly 기본 광고)                        |
| 200  | No filled AD                                                                | 광고 없음                                                 |
| 201  | No filled AD (advertising id unavailable)                                   | AD ID 수집 불가 시 (메시지에 ATT 거부·미결정·Info.plist 누락 등 사유 포함) |
| 400  | The app code is invalid. Please check your app code!                        | App code 불일치 또는 default app code                      |
| 500  | Server error                                                                | Cauly서버 에러                                            |
| -100 | SDK error                                                                   | SDK 에러                                                |
| -200 | Request Failed(You are not allowed to send requests under minimum interval) | 최소요청주기 미달                                             |
