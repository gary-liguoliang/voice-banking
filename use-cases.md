
```
                                   +-----------------------+                  +-------------------+ +---------------------+       +-------------------+
                                     | VoiceServiceProvider  |                  | SCBAuthServiceSCB | | VoiceBankingBackend |       | SCBCoreBankingAPI |
                                     +-----------------------+                  +-------------------+ +---------------------+       +-------------------+
     ------------------------------------------\ |                                        |                      |                            |
     | Grant Permission via Mobile banking app |-|                                        |                      |                            |
     |-----------------------------------------| |                                        |                      |                            |
                                                 |                                        |                      |                            |
                                                 | login                                  |                      |                            |
                                                 |--------------------------------------->|                      |                            |
                                                 |                                        |                      |                            |
                                                 |           access_token + refresh_token |                      |                            |
                                                 |<---------------------------------------|                      |                            |
                                                 |                                        |                      |                            |
                                                 |                                        |                      |                            |
                                                 |                                        |                      |                            |
                                                 |                                        |                      |                            |
-----------------------------------------------\ |                                        |                      |                            |
| Alexa, I want to make a payment to Martin -> |-|                                        |                      |                            |
|----------------------------------------------| |                                        |                      |                            |
                       ------------------------\ |                                        |                      |                            |
                       | what's the amount? <- |-|                                        |                      |                            |
                       |-----------------------| |                                        |                      |                            |
                               ----------------\ |                                        |                      |                            |
                               | 20 dollars -> |-|                                        |                      |                            |
                               |---------------| |                                        |                      |                            |
                                                 |                                        |                      |                            |
                                                 | payment request with 20 dollars with access_token             |                            |
                                                 |-------------------------------------------------------------->|                            |
                                                 |                                        |                      |                            |
                                                 |                                        |                      | payment request            |
                                                 |                                        |                      |--------------------------->|
                                                 |                                        |                      |                            |
                                                 |                                        |                      |           payment response |
                                                 |                                        |                      |<---------------------------|
                                                 |                                        |                      |                            |
                                                 |                                        |     payment response |                            |
                                                 |<--------------------------------------------------------------|                            |
                 ------------------------------\ |                                        |                      |                            |
                 | 20 dollars paid to Martin<- |-|                                        |                      |                            |
                 |-----------------------------| |                                        |                      |                            |
                                                 |                                        |                      |                            |
                                                 
                                                 ```