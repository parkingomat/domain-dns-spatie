# domain-dns-spatie
dns record reader based on https://github.com/spatie/dns


This package contains a class that can fetch DNS records.

```php
use Spatie\Dns\Dns;

$dns = new Dns();

$dns->getRecords('spatie.be'); // returns all available dns records

$dns->getRecords('spatie.be', 'A'); // returns only A records
```

You can use various methods to retrieve info of a record.

```php
$records = $dns->getRecords('spatie.be')

$hostNameOfFirstRecord = $records[0]->host();
```


## Installation

If you do not have [dig](https://linux.die.net/man/1/dig) installed you will need it.

You can install the package via composer:

```bash
composer require spatie/dns
```

## Usage

The class can get these record types: `A`, `AAAA`, `CNAME`, `NS`, `SOA`, `MX`, `SRV`, `TXT`, `DNSKEY`, `CAA`, `NAPTR`.

```php
use Spatie\Dns\Dns;

$dns = new Dns();

$dns->getRecords('spatie.be'); // returns all available dns records

$dns->getRecords('spatie.be', 'A'); // returns only A records
$dns->getRecords('spatie.be', ['A', 'CNAME']); // returns both A and CNAME records
$dns->getRecords('spatie.be', DNS_MX); // returns only MX records
$dns->getRecords('spatie.be', DNS_A | DNS_AAAA); // returns both A and AAAA records
```

`getRecords` will return an array with objects that implement the `Spatie\Dns\Records\Record` interface.

## Working with DNS records

Here's how you can fetch the first A-record of a domain.

```php
$ARecord = $dns->getRecords('spatie.be', 'A')[0];
```

These methods can be called on all records:

- `host()`: returns the host (`spatie.be`)
- `ttl()`: return the time to live (`900`)
- `class()`: returns the class (`IN`)
- `type()`: returns the type (`A`)

When converting a record to a string you'll get a string with all info separated with tabs.

```php
(string)$ARecord // returns `spatie.be.              900     IN      A       138.197.187.74`
```

Some records have additional methods available. For example, records of type A [have an additional `ip()` method](https://github.com/spatie/dns/blob/72bf709a44e19e5d8f0bc7e6c93cf70e7a1b18f3/src/Records/A.php#L6). To know which extra methods there are, check the docblocks above [all record classes](https://github.com/spatie/dns/tree/72bf709a44e19e5d8f0bc7e6c93cf70e7a1b18f3/src/Records) in the source code.

## Using a specific nameserver

You can get records from a specific nameserver.

```php
use Spatie\Dns\Dns;

(new Dns)
    ->useNameserver('ns1.openminds.be') // use ns1.openminds.be
    ->getRecords('spatie.be');
```

## Under the hood

We will use [dig](https://wiki.ubuntuusers.de/dig/) to fetch DNS info. If it is not installed on your system, we'll call the native `dns_get_record()` function.

### Testing

``` bash
composer test
```


## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
