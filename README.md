# voice-banking

**Alexa skill**

```json
{
    "interactionModel": {
        "languageModel": {
            "invocationName": "voice banking",
            "intents": [
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "VoiceBanking",
                    "slots": [],
                    "samples": [
                        "what's my balance",
                        "VoiceBanking show me the money",
                        "VoiceBanking what is my balance"
                    ]
                },
                {
                    "name": "VoicePay",
                    "slots": [
                        {
                            "name": "payee",
                            "type": "AMAZON.SearchQuery",
                            "samples": [
                                "{payee}"
                            ]
                        },
                        {
                            "name": "amount",
                            "type": "AMAZON.NUMBER",
                            "samples": [
                                "{amount} dollors",
                                "{amount}"
                            ]
                        }
                    ],
                    "samples": [
                        "make payment to {payee}",
                        "make a payment to {payee}",
                        "pay gary {amount}",
                        "VoicePay pay {payee} two dollors",
                        "pay {payee} now"
                    ]
                }
            ],
            "types": []
        },
        "dialog": {
            "intents": [
                {
                    "name": "VoicePay",
                    "confirmationRequired": true,
                    "prompts": {
                        "confirmation": "Confirm.Intent.1329575263952"
                    },
                    "slots": [
                        {
                            "name": "amount",
                            "type": "AMAZON.NUMBER",
                            "confirmationRequired": false,
                            "elicitationRequired": true,
                            "prompts": {
                                "elicitation": "Elicit.Slot.644463342953.96768900448"
                            }
                        },
                        {
                            "name": "payee",
                            "type": "AMAZON.SearchQuery",
                            "confirmationRequired": false,
                            "elicitationRequired": true,
                            "prompts": {
                                "elicitation": "Elicit.Slot.712870734623.633553804944"
                            }
                        }
                    ]
                }
            ]
        },
        "prompts": [
            {
                "id": "Elicit.Slot.644463342953.96768900448",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "I heard {payee} , what's the amount?"
                    }
                ]
            },
            {
                "id": "Confirm.Slot.644463342953.96768900448",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "I heard {amount} , is it right?"
                    }
                ]
            },
            {
                "id": "Elicit.Slot.712870734623.633553804944",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "what's the payee?"
                    }
                ]
            },
            {
                "id": "Confirm.Intent.1329575263952",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "are you sure to pay {payee} {amount} dollors?"
                    }
                ]
            },
            {
                "id": "Confirm.Slot.577083544540.980695297411",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "sure to pay to {payee} ?"
                    }
                ]
            },
            {
                "id": "Elicit.Slot.1045182268514.677350757301",
                "variations": [
                    {
                        "type": "PlainText",
                        "value": "what color?"
                    }
                ]
            }
        ]
    }
}
```


**AWS Lambda**

```python

from __future__ import print_function


# --------------- Helpers that build all of the responses ----------------------

def build_speechlet_response(title, output, reprompt_text, should_end_session):
    return {
        'outputSpeech': {
            'type': 'PlainText',
            'text': output
        },
        'card': {
            'type': 'Simple',
            'title': "SessionSpeechlet - " + title,
            'content': "SessionSpeechlet - " + output
        },
        'reprompt': {
            'outputSpeech': {
                'type': 'PlainText',
                'text': reprompt_text
            }
        },
        'shouldEndSession': should_end_session
    }


def build_response(session_attributes, speechlet_response):
    return {
        'version': '1.0',
        'sessionAttributes': session_attributes,
        'response': speechlet_response
    }


# --------------- Functions that control the skill's behavior ------------------

def get_welcome_response():
    """ If we wanted to initialize the session to have some attributes we could
    add those here
    """

    session_attributes = {}
    card_title = "Welcome"
    speech_output = "Welcome to Voice Banking. " \
                    "Please tell me what you want to do by saying, " \
                    "make a payment to someone"
    # If the user either does not reply to the welcome message or says something
    # that is not understood, they will be prompted again with this text.
    reprompt_text = speech_output
    should_end_session = False
    return build_response(session_attributes, build_speechlet_response(
        card_title, speech_output, reprompt_text, should_end_session))


def handle_session_end_request():
    card_title = "Session Ended"
    speech_output = "Thank you for banking with us. " \
                    "Have a nice day! "
    # Setting this to true ends the session and exits the skill.
    should_end_session = True
    return build_response({}, build_speechlet_response(
        card_title, speech_output, None, should_end_session))


def vb(intent, session):
    session_attributes = {}
    speech_output = "I'm sorry to tell you that this feature will be ready in next version!"
    
    return build_response(session_attributes, build_speechlet_response(
        intent['name'], speech_output, None, False))
    

def voice_pay(intent, session):
    session_attributes = {}
    speech_output = "transation has been executed - 4"
    
    if 'payee' in intent['slots']:
        payee = intent['slots']['payee']['value']
        if 'amount' in intent['slots']:
            amount = intent['slots']['amount']['value']
            
            if 'confirmationStatus' in intent and intent['confirmationStatus'] == 'DENIED':
                speech_output = "transaction canceled"
            else:
                if payee.lower() not in ('gary', 'lora', 'laura', 'jessie', 'martin'):
                    speech_output = 'sorry, %s is not in your payee list, please add it from your internet banking app.' % payee
                else:
                    speech_output = "%s dollars paid to: %s" % (amount, payee)
    
    return build_response(session_attributes, build_speechlet_response(
        intent['name'], speech_output, None, False))
    

# --------------- Events ------------------

def on_session_started(session_started_request, session):
    """ Called when the session starts """

    print("on_session_started requestId=" + session_started_request['requestId']
          + ", sessionId=" + session['sessionId'])


def on_launch(launch_request, session):
    """ Called when the user launches the skill without specifying what they
    want
    """

    print("on_launch requestId=" + launch_request['requestId'] +
          ", sessionId=" + session['sessionId'])
    # Dispatch to your skill's launch
    return get_welcome_response()


def dialog_response(attributes, endsession=False):
    """  create a simple json response with card """
    return {
        'version': '1.0',
        'sessionAttributes': attributes,
        'response':{
            'directives': [
                {
                    'type': 'Dialog.Delegate'
                }
            ],
            'shouldEndSession': endsession
        }
    }
    
    
def on_intent(intent_request, session):
    """ Called when the user specifies an intent for this skill """

    print("on_intent requestId=" + intent_request['requestId'] +
          ", sessionId=" + session['sessionId'])
          
    if 'dialogState' in intent_request:
        #delegate to Alexa until dialog sequence is complete
        if intent_request['dialogState'] == "STARTED" or intent_request['dialogState'] == "IN_PROGRESS":
            return dialog_response({}, False)

    intent = intent_request['intent']
    intent_name = intent_request['intent']['name']

    # Dispatch to your skill's intent handlers
    if intent_name == "AMAZON.HelpIntent":
        return get_welcome_response()
    elif intent_name == "AMAZON.CancelIntent" or intent_name == "AMAZON.StopIntent":
        return handle_session_end_request()
    elif intent_name == "VoiceBanking":
        return vb(intent, session)
    elif intent_name == "VoicePay":
        return voice_pay(intent, session)
    else:
        raise ValueError("Invalid intent")


def on_session_ended(session_ended_request, session):
    """ Called when the user ends the session.

    Is not called when the skill returns should_end_session=true
    """
    print("on_session_ended requestId=" + session_ended_request['requestId'] +
          ", sessionId=" + session['sessionId'])
    # add cleanup logic here


# --------------- Main handler ------------------

def lambda_handler(event, context):
    """ Route the incoming request based on type (LaunchRequest, IntentRequest,
    etc.) The JSON body of the request is provided in the event parameter.
    """
    print("event.session.application.applicationId=" +
          event['session']['application']['applicationId'])

    """
    Uncomment this if statement and populate with your skill's application ID to
    prevent someone else from configuring a skill that sends requests to this
    function.
    """
    # if (event['session']['application']['applicationId'] !=
    #         "amzn1.echo-sdk-ams.app.[unique-value-here]"):
    #     raise ValueError("Invalid Application ID")

    if event['session']['new']:
        on_session_started({'requestId': event['request']['requestId']},
                           event['session'])

    if event['request']['type'] == "LaunchRequest":
        return on_launch(event['request'], event['session'])
    elif event['request']['type'] == "IntentRequest":
        return on_intent(event['request'], event['session'])
    elif event['request']['type'] == "SessionEndedRequest":
        return on_session_ended(event['request'], event['session'])


```
