
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


source:

http://weidagang.github.io/text-diagram/

```
object VoiceServiceProvider SCBAuthServiceSCB VoiceBankingBackend SCBCoreBankingAPI

note left of VoiceServiceProvider : Grant Permission via Mobile banking app


VoiceServiceProvider -> SCBAuthServiceSCB: login     
SCBAuthServiceSCB-> VoiceServiceProvider: access_token + refresh_token
space 4

note left of VoiceServiceProvider : Alexa, I want to make a payment to Martin ->

note left of VoiceServiceProvider : what's the amount? <-
note left of VoiceServiceProvider : 20 dollars ->

VoiceServiceProvider -> VoiceBankingBackend: payment request with 20 dollars with access_token
VoiceBankingBackend -> SCBCoreBankingAPI: payment request
SCBCoreBankingAPI-> VoiceBankingBackend: payment response
VoiceBankingBackend  -> VoiceServiceProvider: payment response

note left of VoiceServiceProvider : 20 dollars paid to Martin<-

``` 
