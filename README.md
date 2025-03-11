# Stregatto Starter
### Infra
To setup the infrastructure for this project refer to [stregatto-starter-infra](https://github.com/claranet-it/stregatto-starter-infra).  
If you follow those steps you will find this repo already downloaded inside the provisioned ec2.

### Configure AWS Integration Plugin
Edit `core/cat/plugins/aws_integration/settings.json` with your AWS credentials.
```json
{
    "aws_access_key_id": "<aws_access_key_id>",
    "aws_secret_access_key": "<aws_secret_access_key>",
    "region_name": "<aws_region>",
    "credentials_profile_name": "",
    "endpoint_url": "",
    "iam_role_assigned": false,
    "caller_identity": "<arn_of_the_user_with_bedrock_access>"
}
```

### Configure Amazon Bedrock LLMs Plugin
The official plugin has an issue because it doesn't let you choos "Anthropic Claude 3.5 Sonnet" as LLM but only "Anthropic Claude 3 Sonnet".

To avoid this you have to hardcode the ARN in the `core/cat/plugins/amazon_bedrock_llms/bedrock_llms.py` file at line 30
```python
def get_availale_models(client):
    response = client.list_foundation_models(
        byOutputModality="TEXT", byInferenceType="ON_DEMAND"
    )
    models = defaultdict(list)
    for model in response["modelSummaries"]:
        model_arn = "<model_arn>"
        selected = model["providerName"].lower()
        provider_name = "mistral" if "mistral" in selected else selected
```
