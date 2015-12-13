# Installation

Add this on your `composer.json`:

    "require": {
        "coreproc/enom-sdk-php": "dev-master"
    },
    "repositories": [
        {
            "type": "vcs",
            "url":  "https://git.coreproc.ph/coreproc/enom-sdk-php.git"
        }
    ]
    
# Usage

## Laravel 5.x users

Add this line in the `providers` array in `config/app.php`:

    'providers' => [
        // Other Service Providers
    
        Coreproc\Enom\Providers\EnomServiceProvider::class,
    ],
    
Add these lines in the `facades` array in `config/app.php`:

    'facades' => [
        // Other Facades
    
        'EnomTld' => Coreproc\Enom\Facades\EnomTld::class
        'EnomDomain' => Coreproc\Enom\Facades\EnomDomain::class,
    ],
    
Then run this command to publish the config file:

    php artisan vendor:publish --provider="Coreproc\Enom\Providers\EnomServiceProvider"
    
Set up your credentials on the published file `config/enom.php`:

    <?php
    
    return [
        'userId'   => env('ENOM_USER_ID', ''),
        'password' => env('ENOM_PASSWORD', '')
    ];

You now have access to the facades `Tld` and `Domain` which you can use like so:

    $tlds = EnomTld::getList();
    $domains = EnomDomain::getList();
    
No need to manually set up the Enom client - it's already done. Please see methods of each class below. 
    
## Vanilla PHP

Set up the client

    $enom = new Enom('user-id', 'password');

## TLDs

    $tld = new EnomTld($enom);
    
    try {
        $tld->authorize(['com', 'net', 'io']);
    } catch (Coreproc\Enom\EnomApiException $e) {
        var_dump($e->getErrors());
    }
    
## Methods

Authorize TLDs

    authorize(array $tlds)
    
Remove TLDs

    remove(array $tlds)
    
Get TLD list

    getList()
    
## Domains

    $domain = new EnomDomain($enom);
    
    try {
        $domain->check('example', 'com');
    } catch (Coreproc\Enom\EnomApiException $e) {
        var_dump($e->getErrors());
    }
    
## Methods

    check($sld, $tld)
    
    getNameSpinner($sld, $tld, $options = [])
    
    getExtendedAttributes($tld)
    
    purchase($sld, $tld, $extendedAttributes = [])
    
    getStatus($sld, $tld, $orderId)
    
    getList()
    
    getInfo($sld, $tld)
    
    setContactInformation($sld, $tld, $contactInfo = [])
    
    getContactInformation($sld, $tld)
    
    getWhoIsContactInformation($sld, $tld)

