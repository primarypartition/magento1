# Install Magento 2

## Get Keys

> https://devdocs.magento.com/guides/v2.3/install-gde/prereq/connect-auth.html

## Install ElasticSearch

> https://www.elastic.co/downloads/elasticsearch

## Create Database

> MYSQL DB

## Run Composer

> composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4 pro2

## Install Extensions 

> C:\xampp\php\php.ini

```
extension=php_intl.dll
extension=php_soap.dll
extension=php_xsl.dll
extension=php_sockets.dll
```

## Update Vendor Code

> C:\xampp\htdocs\magento\pro1\lib\internal\Magento\Framework\Image\Adapter\Gd2.php

> C:\xampp\htdocs\magento\pro2\vendor\magento\framework\Image\Adapter\Gd2.php

```
/**
     * Checks for invalid URL schema if it exists
     *
     * @param string $filename
     * @return bool
     */
   
  /** private function validateURLScheme(string $filename) : bool
    {
        $allowed_schemes = ['ftp', 'ftps', 'http', 'https'];
        $url = parse_url($filename);
        if ($url && isset($url['scheme']) && !in_array($url['scheme'], $allowed_schemes)) {
            return false;
        }

        return true;
    }
  **/

  private function validateURLScheme(string $filename) : bool
  {
    $allowed_schemes = ['ftp', 'ftps', 'http', 'https'];
    $url = parse_url($filename);
    if ($url && isset($url['scheme']) && !in_array($url['scheme'], $allowed_schemes) && !file_exists($filename)) {
      return false;
    }

    return true;
  }
```

## Configure Setup

> https://devdocs.magento.com/cloud/before/before-setup-env-install.html

```
php magento setup:install \
  --admin-firstname=John \
  --admin-lastname=Smith \
  --admin-email=jsmith@mail.com \
  --admin-user=admin \
  --admin-password=password1 \
  --base-url=http://magento.local/ \
  --db-host=localhost \
  --db-name=magento \
  --db-user=magento \
  --db-password=magento \
  --currency=USD \
  --timezone=America/Chicago \
  --language=en_US \
  --use-rewrites=1 \
  --backend-frontname=admin
```

## Memory Limit

> ini_set('memory_limit', '-1');

>  php -dmemory_limit=5G bin/magento setup:di:compile

>  php -dmemory_limit=-1 bin/magento setup:di:compile

### Test and Run

> php magento indexer:reindex

> php magento setup:upgrade

> php magento setup:static-content:deploy -f

> php magento cache:flush



## Update Vendor Code

> C:\xampp\htdocs\magento\pro2\vendor\magento\framework\View\Element\Template\File\Validator.php

```
/**
$realPath = $this->fileDriver->getRealPath($path);
**/
$realPath = str_replace('\\', '/', $this->fileDriver->getRealPath($path));  
```

> php magento cache:flush

> php magento module:disable Magento_TwoFactorAuth

> php magento cache:flush


## Change Symlink (Not Required)

> C:\xampp\htdocs\magento\pro2\vendor\magento\framework\View\Element\Template\File\app\etc\di.xml

> Old Value

```
<arguments>
    <argument name="strategiesList" xsi:type="array">
        <item name="view_preprocessed" xsi:type="object">Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink</item>
        <item name="default" xsi:type="object">Magento\Framework\App\View\Asset\MaterializationStrategy\Copy</item>
    </argument>
</arguments>
```

> New Value

```
<arguments>
    <argument name="strategiesList" xsi:type="array">
        <item name="view_preprocessed" xsi:type="object">Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink</item>
        <item name="default" xsi:type="object">Copy</item>
    </argument>
</arguments>
```

> php magento cache:flush

> php magento setup:di:compile

> php magento cache:flush
