## How to Use

## Common methods

TOTP and HOTP objects have the following common methods:

* ```public function at($counter);```: generate an OTP at the specified counter
* ```public function verify($otp, $counter);```: verify if the OTP is valid for the specified counter
* ```public function getProvisioningUri()```: return a provisioning URI

Example:

    $my_otp_object->at(1000); //e.g. will return 123456
    $my_otp_object->verify(123456, 1000); //Will return true
    $my_otp_object->verify(123456, 1001); //Will return false

## Time Based OTP

This OTP object has a specific method:

* ```public function now()```: return an OTP at the current time

Example:

    $my_otp_object->now(); //e.g. will return 123456
    $my_otp_object->verify(123456, time()); //Will return true

### Google Authenticator Compatible

The library works with the Google Authenticator iPhone and Android app, and also
includes the ability to generate provisioning URI's for use with the QR Code scanner
built into the app.

Google only supports SHA-1 digest algorithm, 30 second interval and 6 digits OTP. Other values for these parameters are ignored by the Google Authenticator application.

    <?php
    use MyProject\TOTP;

	$totp = new TOTP;
	$totp->setLabel("alice@google.com")
         ->setDigits(6)
         ->setDigest('sha1')
         ->setInterval(30);
	     ->setSecret("JBSWY3DPEHPK3PXP");

    $totp->getProvisioningUri(); // => 'otpauth://totp/alice%40google.com?algorithm=sha1&digits=6&period=30&secret=JBSWY3DPEHPK3PXP'
    $hotp->getProvisioningUri(); // => 'otpauth://hotp/alice%40google.com?algorithm=sha1&counter=0&digits=6&secret=JBSWY3DPEHPK3PXP&counter=0'

### Working examples

### Compatible with Google Authenticator

Scan the following barcode with your phone, using Google Authenticator

![QR Code for OTP](http://chart.apis.google.com/chart?cht=qr&chs=250x250&chl=otpauth%3A%2F%2Ftotp%2FMy%2520Big%2520Compagny%3Aalice%2540google.com%3Falgorithm%3Dsha1%26digits%3D6%26period%3D30%26secret%3DJBSWY3DPEHPK3PXP%26issuer%3DMy%2520Big%2520Compagny)

Now run the following and compare the output

    <?php
    use MyProject\TOTP;

	$totp = new TOTP;
	$totp->setLabel("alice@google.com")
         ->setDigits(6)
         ->setDigest('sha1')
         ->setInterval(30);
	     ->setSecret("JBSWY3DPEHPK3PXP");

    echo "Current OTP: ". $totp->now();

### Not Compatible with Google Authenticator

The following barcode will not work with Google Authenticator because digest algoritm is not SHA-1, there are 8 digits and counter is not 30 seconds.

![QR Code for OTP](http://chart.apis.google.com/chart?cht=qr&chs=250x250&chl=otpauth%3A%2F%2Ftotp%2FMy%2520Big%2520Compagny%3Aalice%2540google.com%3Falgorithm%3Dsha512%26digits%3D8%26period%3D10%26secret%3DJBSWY3DPEHPK3PXP%26issuer%3DMy%2520Big%2520Compagny)

Now run the following and compare the output

    <?php
    use MyProject\TOTP;

	$totp = new TOTP;
	$totp->setLabel("alice@google.com")
         ->setDigits(8)
         ->setDigest('sha512')
         ->setInterval(10);
	     ->setSecret("JBSWY3DPEHPK3PXP");

    echo "Current OTP: ". $totp->now();
