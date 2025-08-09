# AWS Topics and Queries

* **1) What is IAM in AWS and purpose of IAM**
* **2) Real-world IAM setup for a Java Spring Boot app running on EC2, Lambda** 
* **3) Give a Proper example and explain in detail of save data in S3 bucket using lambda communication between S3 with lambda  Using Java Spring Boot**
* **4) What is AWS Services and Manager Services vs Build in Services and Purpose of Manager Services vs Build in Services , give proper example and explain**
* **5) What is AWS RDS ?**
* **6) What is VPC and What is EC2 in AWS**
* **7) What is the purpose of SQS, SNS  in AWS, Give in detail**
* **8) What is CLoud watch and purpose of CLoud watch in AWS**
* **9) what is ECS ? EKS ? and ECS vs EKS**
* **10) What is Fargate in AWS ?**
    








#### 1) What is IAM in AWS and purpose of IAM

<img width="787" height="286" alt="image" src="https://github.com/user-attachments/assets/d73baf0f-61e7-405d-b9a8-96caba062ec6" />

<img width="831" height="536" alt="image" src="https://github.com/user-attachments/assets/82b3e421-f8f9-446d-85e5-829d088bf08f" />

<img width="805" height="614" alt="image" src="https://github.com/user-attachments/assets/461beb40-c941-4b3c-a233-ed97844f87aa" />

<img width="810" height="656" alt="image" src="https://github.com/user-attachments/assets/767e4583-0828-482a-bd44-774b5223b379" />

<img width="829" height="523" alt="image" src="https://github.com/user-attachments/assets/7e96971a-9c6a-4f4b-add6-d21b2352d39f" />

<img width="816" height="698" alt="image" src="https://github.com/user-attachments/assets/56ce2dcc-b96a-4820-89e8-181a1e3b89c9" />

<img width="813" height="506" alt="image" src="https://github.com/user-attachments/assets/3c751b9a-b083-4f50-af28-b4560b6472a2" />

<img width="801" height="758" alt="image" src="https://github.com/user-attachments/assets/2e50442e-1574-4713-9f26-abdd7c851dc3" />

#### 2) Real-world IAM setup for a Java Spring Boot app running on EC2, Lambda

<img width="841" height="545" alt="image" src="https://github.com/user-attachments/assets/e09c8170-cb80-44b2-882c-cecbe1959f3e" />

<img width="786" height="646" alt="image" src="https://github.com/user-attachments/assets/1fb25431-06a9-4a28-833c-a10379351372" />

<img width="827" height="661" alt="image" src="https://github.com/user-attachments/assets/d6b30996-8cc1-4c3d-95de-2e6fc3e8f4f2" />

<img width="834" height="497" alt="image" src="https://github.com/user-attachments/assets/0b64736d-bb12-452f-adbf-e8b8660ce1a0" />

<img width="819" height="568" alt="image" src="https://github.com/user-attachments/assets/81350d5b-0179-410a-96f7-03004ffd279c" />

<img width="848" height="581" alt="image" src="https://github.com/user-attachments/assets/56708ac0-87aa-46c8-b748-3b3aa710b57d" />

<img width="757" height="472" alt="image" src="https://github.com/user-attachments/assets/0a2ed0e4-d329-4d72-8f65-1c2de820f8b9" />

<img width="729" height="158" alt="image" src="https://github.com/user-attachments/assets/b99a6d4e-1477-4bd2-a888-78806f9634d4" />

<img width="739" height="531" alt="image" src="https://github.com/user-attachments/assets/8ddd021c-2bcc-4148-b4bf-5bec1d3d9a37" />

<img width="773" height="554" alt="image" src="https://github.com/user-attachments/assets/cf531e4d-13d7-4dce-8751-e95008f186ac" />

<img width="758" height="496" alt="image" src="https://github.com/user-attachments/assets/8a430f0f-8e0c-4c19-b0ed-05d8915fd466" />

<img width="768" height="528" alt="image" src="https://github.com/user-attachments/assets/07d5d9c8-c2c8-4e67-a73c-1940904c2eae" />

<img width="754" height="452" alt="image" src="https://github.com/user-attachments/assets/9d36f382-110c-416e-a6b4-72e13b918491" />

<img width="770" height="245" alt="image" src="https://github.com/user-attachments/assets/eff6d611-cecb-487e-bbd5-5099486afd5d" />

#### 3) Give a Proper example and explain in detail of save data in S3 bucket using lambda communication between S3 with lambda  Using Java Spring Boot

<img width="826" height="622" alt="image" src="https://github.com/user-attachments/assets/a3f2aafd-fa5e-4a27-8615-6234a0eb8434" />

<img width="802" height="497" alt="image" src="https://github.com/user-attachments/assets/c6912a0c-ec9a-43a7-bf72-39321415b3d7" />

<img width="806" height="627" alt="image" src="https://github.com/user-attachments/assets/95aa3b7e-9e9f-412d-b944-6100af7d1e29" />

```java
package com.example.lambda;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import software.amazon.awssdk.auth.credentials.EnvironmentVariableCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.PutObjectRequest;

import java.util.Base64;
import java.util.Map;

public class UploadToS3Lambda implements RequestHandler<Map<String, String>, String> {

    private final S3Client s3 = S3Client.builder()
            .region(Region.AP_SOUTH_1)
            .credentialsProvider(EnvironmentVariableCredentialsProvider.create())
            .build();

    private final String bucketName = "my-demo-bucket-veer";

    @Override
    public String handleRequest(Map<String, String> input, Context context) {
        try {
            String fileName = input.get("fileName");
            String base64Content = input.get("content");
            byte[] contentBytes = Base64.getDecoder().decode(base64Content);

            PutObjectRequest putObjectRequest = PutObjectRequest.builder()
                    .bucket(bucketName)
                    .key(fileName)
                    .build();

            s3.putObject(putObjectRequest, software.amazon.awssdk.core.sync.RequestBody.fromBytes(contentBytes));
            return "File uploaded to S3: " + fileName;
        } catch (Exception e) {
            return "Error: " + e.getMessage();
        }
    }
}

```
<img width="828" height="498" alt="image" src="https://github.com/user-attachments/assets/b2681955-3e0a-489d-b4c3-5bc4cc4cd021" />

```java
@RestController
@RequestMapping("/upload")
public class UploadController {

    private final LambdaClient lambdaClient;

    public UploadController() {
        this.lambdaClient = LambdaClient.builder()
                .region(Region.AP_SOUTH_1)
                .credentialsProvider(EnvironmentVariableCredentialsProvider.create())
                .build();
    }

    @PostMapping
    public ResponseEntity<String> uploadToS3(@RequestParam("file") MultipartFile file) throws IOException {
        String fileName = file.getOriginalFilename();
        byte[] content = file.getBytes();
        String base64 = Base64.getEncoder().encodeToString(content);

        String jsonPayload = new ObjectMapper().writeValueAsString(Map.of(
                "fileName", fileName,
                "content", base64
        ));

        InvokeRequest request = InvokeRequest.builder()
                .functionName("upload-to-s3-lambda") // Lambda function name
                .payload(SdkBytes.fromUtf8String(jsonPayload))
                .build();

        InvokeResponse response = lambdaClient.invoke(request);
        String result = response.payload().asUtf8String();

        return ResponseEntity.ok(result);
    }
}

```
<img width="815" height="534" alt="image" src="https://github.com/user-attachments/assets/5d59b643-b1ca-4647-a3bf-8567a9c4db09" />

<img width="873" height="540" alt="image" src="https://github.com/user-attachments/assets/be8643ac-e5e8-48c8-b24d-8145ef2afe9d" />

#### 4) What is AWS Services and Manager Services vs Build in Services and Purpose of Manager Services vs Build in Services , give proper example and explain

<img width="808" height="423" alt="image" src="https://github.com/user-attachments/assets/7300fda5-2f2b-487d-81b1-3360548cfd13" />

<img width="817" height="744" alt="image" src="https://github.com/user-attachments/assets/75901ab7-ec00-4c3a-976f-631fe5b56455" />

<img width="776" height="718" alt="image" src="https://github.com/user-attachments/assets/6544ce64-131d-46ca-ba71-78f9a0c10b26" />

<img width="812" height="669" alt="image" src="https://github.com/user-attachments/assets/72b82199-c4b6-46c3-9ab7-52c20da2e8b2" />

<img width="825" height="160" alt="image" src="https://github.com/user-attachments/assets/9e2fc830-3492-411b-89a8-49de5e627497" />

<img width="883" height="514" alt="image" src="https://github.com/user-attachments/assets/b6f78a0b-0e83-45da-a870-243abcbd211f" />

<img width="830" height="271" alt="image" src="https://github.com/user-attachments/assets/ed5f0709-4803-4ce1-adc0-316ed9596428" />


#### 5) What is AWS RDS ?

<img width="830" height="324" alt="image" src="https://github.com/user-attachments/assets/de9e31e4-c11b-4ca0-8c9e-d7ba3f0d5a86" />

<img width="822" height="440" alt="image" src="https://github.com/user-attachments/assets/5814a0e8-d13d-4861-8be3-cea2ec797c21" />

<img width="822" height="440" alt="image" src="https://github.com/user-attachments/assets/3a0fa400-9d5e-410e-b0fc-8f9557ce852c" />

<img width="829" height="422" alt="image" src="https://github.com/user-attachments/assets/4dfb8105-5cfa-445d-aab0-d491ff17aac5" />

<img width="823" height="679" alt="image" src="https://github.com/user-attachments/assets/490a0cc1-61b8-4db5-882c-e23398717c73" />

<img width="803" height="595" alt="image" src="https://github.com/user-attachments/assets/23cacd1c-ff15-49a1-98df-e88d908bdcf3" />

<img width="754" height="312" alt="image" src="https://github.com/user-attachments/assets/1b73ac8d-473a-4b9c-acb3-7408be7a9110" />

<img width="824" height="677" alt="image" src="https://github.com/user-attachments/assets/7730a50a-ea8b-4a3a-be08-1b4e73fd985b" />

<img width="852" height="603" alt="image" src="https://github.com/user-attachments/assets/295fe2c2-a74d-4d8e-9bbc-aa2e27e022dc" />

<img width="801" height="643" alt="image" src="https://github.com/user-attachments/assets/342e9acb-da91-46fe-9dc0-5d83d3b7694d" />

<img width="817" height="648" alt="image" src="https://github.com/user-attachments/assets/6112ee3c-b754-49b7-99b0-4056f6cb3c10" />

<img width="789" height="441" alt="image" src="https://github.com/user-attachments/assets/7671e88d-f79d-453f-80e4-708eef226c5d" />

<img width="808" height="383" alt="image" src="https://github.com/user-attachments/assets/e682ec89-5fec-419c-a532-704e072ef128" />

#### 6) What is VPC and What is EC2 in AWS

<img width="832" height="595" alt="image" src="https://github.com/user-attachments/assets/6dc7f74b-3c5a-44f0-bad7-145f62d5a13f" />

<img width="807" height="545" alt="image" src="https://github.com/user-attachments/assets/0c1f7e98-17df-4619-9f67-b2f77334838a" />

<img width="845" height="470" alt="image" src="https://github.com/user-attachments/assets/107542c6-c824-4401-b658-6e6ab4305a2e" />

<img width="610" height="422" alt="image" src="https://github.com/user-attachments/assets/18d6c094-22e8-4ce7-8e4d-92f652d0820e" />

#### 7) What is the purpose of SQS, SNS  in AWS, Give in detail

<img width="815" height="462" alt="image" src="https://github.com/user-attachments/assets/76621d9d-0203-4372-a842-19b1fdd2cb6b" />

<img width="806" height="714" alt="image" src="https://github.com/user-attachments/assets/2f5eb4a0-b04d-4a7d-90a0-c5f5812b6eaa" />

<img width="802" height="573" alt="image" src="https://github.com/user-attachments/assets/46a1c63d-9484-43e8-940a-64a6c49712b1" />

<img width="819" height="615" alt="image" src="https://github.com/user-attachments/assets/b644aaa3-d86a-4f2d-bae5-fb088a0ce3f2" />

<img width="818" height="562" alt="image" src="https://github.com/user-attachments/assets/660f0aba-d43d-44ab-96a0-e83ca5f48820" />

<img width="794" height="732" alt="image" src="https://github.com/user-attachments/assets/38c33b77-7471-4da4-9eb9-e2324015d484" />

<img width="790" height="604" alt="image" src="https://github.com/user-attachments/assets/e8828fc8-61be-429e-840c-019e3c7cf3fb" />

<img width="797" height="664" alt="image" src="https://github.com/user-attachments/assets/13d1d34f-aa9f-43e2-b064-eef2b542e836" />

#### 8) What is CLoud watch and purpose of CLoud watch in AWS**

<img width="827" height="755" alt="image" src="https://github.com/user-attachments/assets/dd907066-7060-47fc-97e5-b5a1c17eecd1" />



#### 9) what is ECS ? EKS ? and ECS vs EKS


#### 10) What is Fargate in AWS ?

