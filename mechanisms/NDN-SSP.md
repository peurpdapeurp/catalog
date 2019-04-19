
# NDN-SSP

* How does device introduce itself to the infrastructure or peer?
  The device sends a sign-on request to a controller. The controller serves as a local trust anchor. If the device's security credentials are valid (i.e. its pre-shared secret keys are correct), then the controller provisions new security credentials to allow the device to securely communicate within the network that the controller serves as a trust anchor for.

* What, if any, proof of possession mechanism is there?
  Signatures on messages based on security credentials such as a pre-shared symmetric key / pre-shared asymmetric key pair are used to guarantee authenticity of messages during the sign-on process.

* Is access to other IP-based devices required in order to fully onboard the device?
  No. A device can be signed on to a controller locally through a link layer protocol like BLE.

* What form of credential is installed on the device?
  A pre-shared asymmetric key and a pre-shared symmetric key.

* Is full Internet access required for onboarding?
  No.

* Who becomes the root of trust at the end of onboarding (if any)
  The controller serves as a local root of trust.

* Could/Is the resulting credential be used for application identity?
  Yes, the device gets an asymmetric key pair that is certified by the controller. This public private key pair can be used to sign all further messages sent out by the device, so that any other devices in the same network where the controller is serving as a trust anchor can verify the device's messages' integrity.

* What happens if the box gets reset?
  Currently, if the controller gets reset, all memory of signed-on devices is wiped. Currently, if the device gets reset, all memory of having signed-on is wiped. The sign-on protocol is still in development, so this will have to be addressed.

* How can transfer of ownership occur?
  There is no notion of transfer of ownership as of now. Once a controller is selected to be a root of trust for the network, 

* How can transfer to a different cloud service (if applicable) occur?
  The sign-on protocol is meant to support an IoT system that is not dependent on the cloud.

* What sort of manufacturing requirements are there?
  There must be an out-of-band mechanism for the device to share the public key of its initial asymmetric key pair with the controller, to allow the controller to verify messages from the device. This means that the manufacturer will likely have to attach some kind of easily scannable representation of the public key (i.e. a QR code) to their devices, in order to allow users to easily sign on their devices to a controller.

* What sort of crypto requirements are there?
  There are several variants of the protocol depending on the capabilities of the device being signed on, but for the basic version of the protocol which assumes that the device has minimal capabilities, the requirements are these:
  ** The device should be able to generate an asymmetric key pair for a diffie hellman exchange (this key pair does not need to be high entropy, as the asymmetric key pair for diffie hellman will be used for a short period of time, only during the sign-on process itself, which should only take a few seconds)
  ** The device should be able to generate and verify asymmetric key signatures (i.e. ECDSA with secp256r1 curve)
  ** The device should be able to generate and verify symmetric key signatures (i.e. HMAC with sha256)
  ** The controller should be able to generate an asymmetric key pair for diffie hellman exchange (this key pair does not need to be high entropy, as the asymmetric key pair for diffie hellman will be used for a short period of time, only during the sign-on process itself, which should only take a few seconds)
  ** The controller should be able to generate a high entropy asymmetric key pair for long term use for the device
  ** The controller should be able to generate and verify asymmetric key signatures (i.e. ECDSA with secp256r1 curve)
  ** The controller should be able to generate and verify symmetric key signatures (i.e. HMAC with sha256)

* Reference link
https://github.com/named-data-iot/ndn-lite/wiki/Security-Bootstrapping