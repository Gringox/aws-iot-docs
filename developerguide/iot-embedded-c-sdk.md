# Using the AWS IoT Device SDK for Embedded C<a name="iot-embedded-c-sdk"></a>

This section describes how to run the AWS IoT Device SDK for Embedded C\.

## Install the AWS IoT Device SDK for Embedded C<a name="install-embedded-c-sdk"></a>

The AWS IoT Device SDK for Embedded C is generally targeted at resource constrained devices that require an optimized C language runtime\. You can use the SDK on any operating system and host it on any processor type \(for example, MCUs and MPUs\)\. If you have more memory and processing resources available, we recommend that you use one of the higher order AWS IoT Device and Mobile SDKs \(for example, C\+\+, Java, JavaScript, and Python\)\.

In general, the AWS IoT Device SDK for Embedded C is intended for systems that use MCUs or low\-end MPUs that run embedded operating systems\. For programming examples in the documentation, we use Raspberry Pi running embedded Linux\.

**Example**  

1. Download the AWS IoT Device SDK for Embedded C to your Raspberry Pi from [GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C)\.

   ```
   git clone https://github.com/aws/aws-iot-device-sdk-embedded-c.git -b release
   ```

   This creates a directory named `aws-iot-device-sdk-embedded-c` in the current directory\.

1. Download mbed TLS to your Raspberry Pi from the [mbed TLS website](https://tls.mbed.org/download)\.

1. Navigate into the `/home/pi/Downloads` directory\. Expand the `mbedtls-versionNumber-apache.tgz` file for the latest version by using the following command\.

   ```
   tar -xvf mbedtls-versionNumber-apache.tgz
   ```

1. Copy the contents of `mbedtls-versionNumber` into the `aws-iot-device-sdk-embedded-C/external_libs/mbedTLS` directory using the following command\.

   ```
   mv ~/Downloads/mbedtls-versionNumber/* ~/aws-iot-device-sdk-embedded-c/external_libs/mbedTLS
   ```

## Sample app configuration<a name="iot-c-sdk-app-config"></a>

The AWS IoT Device SDK for Embedded C includes sample applications for you to try\. For simplicity, this tutorial uses the `subscribe_publish_sample` application, which illustrates how to connect to the AWS IoT Core message broker and subscribe and publish to MQTT topics\.

1. Copy the certificate, private key, and root CA certificate you created in [Create an AWS IoT thing for your Raspberry Pi](sdk-tutorials.md#iot-sdk-create-thing) into the `aws-iot-device-sdk-embedded-C/certs` directory\.
**Note**  
Device and root CA certificates are subject to expiration or revocation\. If your certificates expire or are revoked, you must copy a new CA certificate or private key and device certificate onto your device\.

1. You must configure the sample with your personal AWS IoT Core endpoint, private key, certificate, and root CA certificate\. Navigate to the `aws-iot-device-sdk-embedded-c/samples/linux/subscribe_publish_sample` directory\. 

   If you have the AWS CLI installed, you can use the aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS command to find your personal endpoint URL\. If you don't have the AWS CLI installed, open your [AWS IoT console](https://console.aws.amazon.com/iot/home)\. From the navigation pane, choose **Manage**, and then choose **Things**\. Choose the IoT thing for your Raspberry Pi, and then choose **Interact**\. Your endpoint is displayed in the ** HTTPS** section of the thing details page\.

1. Open the `aws_iot_config.h` file and, in the `Get from console` section, update the values for the following:  
AWS\_IOT\_MQTT\_HOST  
Your personal endpoint\.  
AWS\_IOT\_MY\_THING\_NAME  
Your thing name\.  
AWS\_IOT\_ROOT\_CA\_FILENAME  
Your root CA certificate file name \(no path\)\.  
 AWS\_IOT\_CERTIFICATE\_FILENAME  
Your certificate file name \(no path\)\.  
AWS\_IOT\_PRIVATE\_KEY\_FILENAME  
Your private key file name \(no path\)\.

   For example:

   ```
   // Get from console
   // =================================================
   #define AWS_IOT_MQTT_HOST              "a22j5sm6o3yzc5.iot.us-east-1.amazonaws.com"
   #define AWS_IOT_MQTT_PORT              8883
   #define AWS_IOT_MQTT_CLIENT_ID         "MyRaspberryPi"
   #define AWS_IOT_MY_THING_NAME          "MyRaspberryPi"
   #define AWS_IOT_ROOT_CA_FILENAME       "root-CA.crt"
   #define AWS_IOT_CERTIFICATE_FILENAME   "device.pem.crt"
   #define AWS_IOT_PRIVATE_KEY_FILENAME   "private.pem.key"
   // =================================================
   ```

1. Check to see if you have the development tools installed on your device by using this command\.

   ```
   gcc --version
   ```

   If you see the version information for the compiler, you can continue to the next section\.

   If you get an error or don't see any compiler information, you'll need to install the `build-essential` package using this command\.

   ```
   sudo apt-get install build-essential
   ```

   Run the gcc \-\-version command again and confirm that the build tools have been installed and that you are ready to continue\.

## Build and run the sample application<a name="iot-c-sdk-app-run"></a>

**To run the AWS IoT Device SDK for Embedded C sample applications**

1. Use the makefile in the `aws-iot-device-sdk-embedded-c/samples/linux/subscribe_publish_sample` directory to compile the `subscribe_publish_sample` app\. From the `aws-iot-device-sdk-embedded-c/samples/linux/subscribe_publish_sample` directory, enter this command to generate the executable app file\.

   ```
   make -f Makefile
   ```

1. Run the `subscribe_publish_sample` app with this command\.

   ```
   ./subscribe_publish_sample
   ```

   You should see output similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/successful-run.png)

Your Raspberry Pi is now connected to AWS IoT using the AWS IoT Device SDK for Embedded C\.

You can also use the AWS IoT console to view the MQTT messages that the sample app is publishing\. For information about how to use the MQTT client in the [AWS IoT console](https://console.aws.amazon.com/iot/home), see [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md) \.