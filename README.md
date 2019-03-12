# aws-static-website

Static website, hosted into AWS S3, deployed into AWS CloudFront CDN with enabled logging, added readable domain name (AWS Route53) and certificate (AWS ACM).

![Static website architecture](https://raw.githubusercontent.com/alexandre-senko/aws-static-website/master/AWS_static_website_architecture.jpg)

Your content - index.html file and others (*.html, *.js, *.css etc) being hosted into the "RootBucket" S3 bucket. This bucket, through the bucket policy protection connected to the CloudFront origin identity. CloudFron acts as a CDN with additinal restrictions - only HTTPS traffic with GET and HEAD requests allowed. "RootBucket" is a source for this CDN. Default expiration time is 24 hours.

"RootBucket" must have the same name, as your domain name, registered or migrated into Route53 - DOMAIN_NAME. This name added into Route53 HostedZone as an alias of a CloudFront endpoint. Empty "WWWBucket" S3 bucket with the redirect configuration is needed to redirect WWW.DOMAIN_NAME requests to DOMAIN_NAME. WWW.DOMAIN_NAME has a CNAME Ruote53 recordset for the "WWWBucket" URL.

CDN logs stored into the "WebsiteLogsBucket" S3 bucket.  

## Getting Started

These instructions will get you a copy of the CloudFormation, which can be deployed into your AWS account.

### Prerequisites

To use this template, you should have a Route53 hosted zone for your domain name. It could be ordered either [directly into Route53](https://docs.aws.amazon.com/en_us/Route53/latest/DeveloperGuide/domain-register.html) or migrated from [external DNS](https://docs.aws.amazon.com/en_us/Route53/latest/DeveloperGuide/domain-transfer-to-route-53.html).
Then, you should add your SSL certificate into the AWS ACM service. Your certificate should be [uploaded](https://docs.aws.amazon.com/en_us/acm/latest/userguide/import-certificate.html) in N.Virginia region, or [requested from AWS ACM](https://docs.aws.amazon.com/en_us/acm/latest/userguide/gs-acm-request-public.html) in this region. This is important: only [N.Virginia](https://docs.aws.amazon.com/acm/latest/userguide/acm-regions.html) certificates allowed for the CloudFront. Certificate should sign DOMAIN_NAME and WWW.DOMAIN_NAME.


### Deployment

In order to [deploy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) "infrastructure.yaml" CloudFormation template, your should specify input parameters:

RootDomainName - Your website domain name
HostedZoneName - Name of a Route53 HostedZone with your website domain name
SSLCertificateARN - [ARN](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) of your certificate, imported into ACM
PricingClass - CloudFormation [pricing class](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PriceClass.html)

Please note, that deployment process may takes up to 40 min. Main time consuming part is a CloudFront deployment.

After sucessfully creating CloudFormation stack, you should upload your content into the "RootBucket".  


## Built With

Used following snippets of a AWS CloudFromation templates 
* [Route53 with the CloudFront distribution](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-route53.html#w2ab1c17c23c81c11)
* [S3 bucket, configured for the static website hosting](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-s3.html#scenario-s3-bucket-website-customdomain)
* [CloudFront with the S3 origin](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-cloudfront.html#scenario-cloudfront-s3origin)

## Authors

* **ALexandre Senko** - [alexandre-senko](https://github.com/alexandre-senko/)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details


