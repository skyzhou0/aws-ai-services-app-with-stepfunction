# aws-ai-services-app-with-stepfunction
aws-ai-services-app-with-stepfunction

The information outlined in the repo has been inspired by Adam Vincent, and some of the materials are taken from his demo. All credits go to Adam.

## What is objective?

The broad objective is to build a blueprint to leveage AWS AI services and AWS Step Function to build AI Applications. In the current application, we aim to build a transcribe service that can turn Audio in English to Spanish. The steps to achieve this are:

1. We expect customer to provide Audio file in mp3 format, i.e. similar to the audio files in the 'audio' folder.
2. The uploaded audio file action to S3 bucket trigger the Lambda function in the "lambda-trigger-stepfunction-function" folder. In essence, it activates the step function.
3. The first task in the step function is to transcribe the Audio file from English to Spainish and persist such Audio on S3. 
4. The next function in the Stat Machine is the "transcribe-status-function" which checks the status of the transcribe job. 
5. Once the transcribe complete, Step function kicks off both Translate job i.e. to Spanish (es) or Chines (zh) and Comprehend job to see if the context contains postive or negative sentiment.
6. The tanslate text will be turn into an Audio file using Amazon Polly 
	```bash
	# for Chinese cn
	LanguageCode='cmn-CN'
	VoiceId='Zhiyu'
	# for Spanish
	LanguageCode='es-ES'
	VoiceId='Lucia'
	```

## Application Access
Step function pipepline needs to assume an IAM role (let's name it "step-function-execution-role"), such role should allow Step function to execute Lambda functions. A (in-line) policy permission can be found in "step-function-execution-policy.json".

## Reference:
Amazon Polly: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/polly.html#Polly.Client.start_speech_synthesis_task
Amazon Translate: https://boto3.amazonaws.com/v1/documentation/api/1.9.42/reference/services/translate.html
