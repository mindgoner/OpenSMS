# OpenSMS (first release date: 01.08.2024)

OpenSMS is a PHP library that allows you to send SMS messages from your Symfony or Laravel projects with ease. This library is designed to be simple, reliable, and highly configurable.

## Features

- Send SMS messages with minimal configuration.
- Support for multiple SMS gateways.
- Easy integration with Symfony and Laravel applications.
- Well-documented and easy to use.
- Supports both peer-to-peer (DirectSMS) and server-queued (ServedSMS) architectures.

## Installation

You can install OpenSMS via Composer:

```bash
composer require mindgoner/opensms
```
## Usage



### Symfony

DirectSMS (Peer-to-Peer) / ServeSMS (Client-server)

Send SMS messages directly from your application to local (LAN) OpenSMSHardware device or using OpenSMServer in WAN:
```yaml
# config/services.yaml
services:
    App\Service\SmsService:
        arguments:
            $arch: 'p2p'
            $host: '192.168.0.100'
            $apiToken: 'API_TOKEN_HERE'
```

```php
// src/Service/SmsService.php
namespace App\Service;

use Mindgoner\OpenSMS\DirectSMS;

class SmsService
{
    private $directSms;

    public function __construct(string $host, string $apiToken)
    {
        $this->directSms = new DirectSMS($host, $apiToken);
    }

    public function sendSms(string $recipient, string $message): void
    {
        $this->directSms->send($recipient, $message);
    }

}

```

```php
// src/Controller/SmsController.php
namespace App\Controller;

use App\Service\SmsService;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class SmsController extends AbstractController
{
    private $smsService;

    public function __construct(SmsService $smsService)
    {
        $this->smsService = $smsService;
    }

    /**
     * @Route("/send-sms", name="send_sms")
     */
    public function sendSms(): Response
    {
        $recipient = 'recipient_number';
        $message = 'Your message here';

        $this->smsService->sendSms($recipient, $message);

        return new Response('SMS sent successfully');
    }
}

```

### Laravel
DirectSMS (Peer-to-Peer)
Send SMS messages directly from your application to local (LAN) OpenSMSHardware device:
```php
use Mindgoner\OpenSMS\DirectSMS;

$sms = new DirectSMS("192.168.0.100", "API_TOKEN_HERE");
$sms->send('recipient_number', 'Your message here');
```
ServedSMS (Server-Queued)
Queue SMS messages to be sent via OpenSMSServer (WAN) to OpenSMSHardware device:
```php
use Mindgoner\OpenSMS\ServedSMS;

$sms = new ServedSMS("41.251.72.24", "API_TOKEN_HERE");
$sms->queue('recipient_number', 'Your message here');
```

## Related Projects

### OpenSMSServer

A [Laravel application](https://github.com/mindgoner/OpenSMSServer) that acts as a gateway for sending SMS messages from IoT modules located in different networks.

### OpenSMSHardware

[Firmware](https://github.com/mindgoner/OpenSMSHardware) for various microcontrollers to process API requests and send SMS messages.


### License

This project is licensed under the MIT License - see the LICENSE file for details.

### Summary

The updated `README.md` for OpenSMS now includes information on how to use both the DirectSMS (peer-to-peer) and ServedSMS (server-queued) functionalities, providing clear examples for Symfony and Laravel applications.
