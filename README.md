# AddressZen Symfony Bundle

Configure your AddressZen API key once and inject the [PHP SDK](https://packagist.org/packages/addresszen/sdk) client into your services. Symfony 6.4, 7 or 8, PHP 8.2 or later.

```bash
composer require addresszen/symfony-bundle
```

Symfony Flex registers the bundle. Without Flex, add it to `config/bundles.php`:

```php
AddressZen\Symfony\AddressZenSymfonyBundle::class => ['all' => true],
```

```yaml
# config/packages/addresszen.yaml
addresszen:
    api_key: '%env(ADDRESSZEN_API_KEY)%'
    management_key: '%env(ADDRESSZEN_MANAGEMENT_KEY)%' # only for key, licensee and config operations
```

```php
use AddressZen\Client;
use AddressZen\Types\UsaVerifyMatch;

final class CheckoutController
{
    public function __construct(private readonly Client $client) {}

    public function isDeliverable(string $address): bool
    {
        return $this->client->addressVerify(['query' => $address])->result instanceof UsaVerifyMatch;
    }
}
```

Inject `AddressZen\Client`, or `AddressZen\Contracts\ClientContract` to swap in a test double. Each API operation is one method on it, such as `$client->addressVerify($body)`.

Requests go through the `Psr\Http\Client\ClientInterface` service when your application defines one, which `symfony/http-client` does once `nyholm/psr7` is installed, so `framework.http_client` timeouts and retries apply. Otherwise the SDK discovers an installed PSR-18 client.

| Option | Purpose |
| --- | --- |
| `api_key` | Required. Your API key or a licensee key |
| `management_key` | Your Management Key for account operations |
| `base_uri` | Defaults to `https://api.addresszen.com/v1` |
| `http_client` | Service id of a PSR-18 client to use instead of the default |
| `headers` | Headers sent with every request |

[Documentation](https://docs.addresszen.com/docs/sdks/php/symfony) covers configuration, testing with a mock client and the SDK itself.

## License

SEE LICENSE IN LICENSE
