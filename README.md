# magento2-module-csp
A module for CSP amends as documented here: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/security/content-security-policies.html#configure-a-modules-csp-mode

By default this module will set the CSP mode to `restrict`
It is strongly reccommended that you install this module on a development/staging site and test thoroughly before taking live.
If unsure see [disabling restrict mode](#disabling-restrict-mode)

This module includes some common exceptions that are used frequently across Magento 2 sites.
To add an exclusion simply update the `etc/csp_whitelist.xml`
We have also included some additional sample configuration that can be added to `etc/csp_whitelist.xml` if you use that service, or have that module installed.
See [whitelist templates](#whitelist-templates) for more information

## Installation
The easiest way to install this module is by directly clone the repo.
*N.B* this module is not distributed as a composer package as you will need to edit `etc/csp_whitelist.xml` which would not be possible for a composer pacakge.

1. Clone the module: `git clone https://github.com/zero1limited/magento2-module-csp.git app/code/Zero1/Csp && rm -rf app/code/Zero1/Csp/.git`
2. Enable the module: `php bin/magento module:enable Zero1_Csp`
3. Run magento upgrade: `php bin/magento setup:upgrade`


## Disabling Restrict Mode
If you want to run your store in the default "report_only" mode, simply change the code in the file `etc/config.xml` from  
```xml
<default>
    <csp>
        <mode>
            <storefront>
                <report_only>0</report_only>
            </storefront>
            <admin>
                <report_only>0</report_only>
            </admin>
        </mode>
    </csp>
</default>
```
to 
```xml
<default>
    <csp>
        <mode>
            <storefront>
                <report_only>1</report_only>
            </storefront>
            <admin>
                <report_only>1</report_only>
            </admin>
        </mode>
    </csp>
</default>
```

## Whitelist templates
All our templates are stored [/var/csp_templates](/var/csp_templates) to use the configuration copy each element into the correct node within `etc/csp_whitelist.xml`

- [Ebizmarts_MailChimp](/var/csp_templates/Ebizmarts_MailChimp.xml)

## Additional Configuration

### Unsafe inline
If you see the report: `The Content-Security-Policy directive 'frame-ancestors' does not support the source expression ''unsafe-inline''`
Consider adding:
```xml
<default>
    <csp>
        <policies>
            <storefront>
                <frame-ancestors>
                    <inline>0</inline>
                </frame-ancestors>
            </storefront>
        </policies>
    </csp>
</default>
```
to `app/code/Zero1/Csp/etc/config.xml`
