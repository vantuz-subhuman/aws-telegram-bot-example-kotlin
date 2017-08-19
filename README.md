# AWS-Telegram-Bot-Example-Kotlin
- Telegram bot
- Webhook on [AWS-Lambda]
- Implemented in [Kotlin] with [Gradle]
- [gradle-aws-plugin] is used to install and update lambdas
- [jackson-module-kotlin] is used to convert Kotlin data objects to and from strings
- [Fuel] is used to send Telegram requests

## About
Just an example (and a template) of how easily you can create a Telegram bot
hosted on AWS-Lambda service as webhook.

Result bot can't do much, it just sends you its version in response to `/version`
message, or echoes any other message.

## Usage
Gradle plugin allows you to easily create and update the lambda function.
In order to use it, you have to have an Amazon AWS account. Then you need to
[create an access key](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html)
for your account and to [configure the AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
on your dev machine. This will give you the "profile name" that you need to specify in your gradle script:
```
aws {
    profileName = 'default'
}
```
Then you can configure your future (or existing) lambda parameters:
```
def lambda_name_test = "MyBotTestLambdaName"
def lambda_name_production = "MyBotProductionLambdaName"
def lambda_role = "arn:aws:iam::${aws.accountId}:role/lambda-role"
def lambda_handler = "com.vsubhuman.telegram.kt_example.Main"
```
**Note!** That if you already have a prepared lambda, then you only have to specify its name!
All the other properties are only required for lambda installation.

**Note2!** The specified role must exist, or build will fail!
You may manage roles in the [IAM console](https://console.aws.amazon.com/iam/home).

Handler points to the class that implements the lambda interface
and that will be used as the entry point.

### Create

When all parameters are set you may call commands:
```
gradle install_lambda
```
or
```
gradle install_lambda_production
```
These commands will attempt to create a new lambda with either
test or production name respectively, and to deploy current code.

### Update

After you already got an existing lambda and you just want to update
its code, call:
```
gradle deploy
```
or
```
gradle deploy_production
```
In order to build an uberjar and to upload it to either test or production
lambda respectively.

## bot_token

**Note!** That your lambda must have an environment property named `bot_token`
with the value of the telegram token of your either test or production bot.
You may find and configure youe lambda function in the console: https://console.aws.amazon.com/lambda/home

## Fire up

Note that when you call your bot first time after an update, or ufter a long time
pause - it needs a time to fire up your lambda environment. Amazon actually starts up
a lambda only when it's called and kills it when it not called for some time.

So give it a few seconds.

[AWS-Lambda]: https://aws.amazon.com/lambda/
[Kotlin]: https://kotlinlang.org/
[Gradle]: https://gradle.org/
[gradle-aws-plugin]: https://github.com/classmethod/gradle-aws-plugin
[jackson-module-kotlin]: https://github.com/FasterXML/jackson-module-kotlin
[Fuel]: https://github.com/kittinunf/Fuel