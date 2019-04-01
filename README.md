# alexa-sdk-nodejs-openfaas-example
Example of Alexa SDK with Node.js and OpenFaaS

## Example

William is a skill which is based upon the [Hello World](https://github.com/alexa/skill-sample-nodejs-hello-world) of the [Alexa SDK for Node.js](https://github.com/alexa/alexa-skills-kit-sdk-for-nodejs)

### Create your skill

* Create a Custom Skill
* Use a HTTPS URL with your OpenFaaS gateway URL and the function name:

```
https://my-openfaas.com/function/william-skill
```

If you don't want to setup your own TLS for your OpenFaaS server, or if it's behind a firewall then use the ngrok tool from ngrok.com

### Setup your intent

Go to the JSON editor and paste in the following:

```json
{
    "interactionModel": {
        "languageModel": {
            "invocationName": "william",
            "intents": [
                {
                    "name": "AMAZON.FallbackIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": [
                        "what can I say"
                    ]
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                },
                {
                    "name": "HelloWorldIntent",
                    "slots": [],
                    "samples": [
                        "hi",
                        "hello there"
                    ]
                }
            ],
            "types": []
        }
    }
}
```

### Deploy William

* Now edit william/stack.yml and replace "alexellis2" for your own Docker Hub username

* Pull the node10-express template:

```
faas-cli template store pull node10-express
```

* Deploy William via the `william-skill` function

```
faas-cli up
```

### Ask William a question

You can now ask William a question in the AWS Alexa console:

```
ask william hello
```

And here's the result!

![](./console-hello.png)

## Extend the example

See if you can get William to say Hello in another language such as Spanish: Hola!

Now edit the `HelloWorldIntentHandler` in `william-skill/handler.js` and run `faas-cli up`

### Adapting the Skills SDK

This was simply a case of adding the following:

```js
module.exports = function(event, context) {

  const skillHandler = skillBuilder
  .addRequestHandlers(
    LaunchRequestHandler,
    HelloWorldIntentHandler,
    HelpIntentHandler,
    CancelAndStopIntentHandler,
    SessionEndedRequestHandler
  )
  .addErrorHandlers(ErrorHandler)
  .lambda();

  skillHandler(event.body, context, function(err, res) {
    console.log(err, res);

    context.status(200).succeed(res);
  });

}
```

For additional help please see the [Alexa SDK documentation](https://github.com/alexa/alexa-skills-kit-sdk-for-nodejs).
