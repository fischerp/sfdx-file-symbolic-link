# Salesforce DX project

The idea of this project is proof that File symbolic links stopped working after the salesforcedx version 46.6.0 (mac os, linux).

Steps to reproduce:
* Create a new SFDX project
* Create a scratch org
* Login on this scratch org to create a class called helloWorld.cls
* On VsCode, pull changes to your local project
* Move this class and its metadata to a folder on your desktop, I suggested ".git_externals" as your folder name
* Create a file symbolic link from your desktop .../Desktop/.git_externals/helloWorld.cls to your project class folder (...force-app/main/default/classes)
* Create a file symbolic link from your desktop .../Desktop/.git_externals/helloWorld.cls-meta.xml to your project class folder (...force-app/main/default/classes/helloWorld.cls-meta.xml)
* Change the content of the hello.cls class, can be an empty space
Push the code to your org
* If if fails, execute this command "sfdx plugins:install salesforcedx@46.6.0" and it will work
* To make it fail again, execute "sfdx plugins:install salesforcedx@46.9.0" or "sfdx plugins:install salesforcedx@46.10.2"


# Symbolic links

Create the following files outside of your project, then create a file symbolic link.

## helloWorld.cls

```
public class helloWorld {
    public void method() {
        System.debug('File symbolic link error after salesforcedx@46.6 - mac/linux');
        System.debug('Execute "sfdx plugins:install salesforcedx@46.6.0" to fix');
        System.debug('Execute "sfdx plugins:install salesforcedx@46.10.2" or newer to make it fail again');
    }
}
```

## helloWorld.cls-meta.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<ApexClass xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>46.0</apiVersion>
    <status>Active</status>
</ApexClass>
```

## Create the Symbolic link
Command example (Mac OS):
```bash
ln -f -s .../git/.classesOut/helloWorld.cls .../git/demo/force-app/main/default/classe
s/helloWorld.cls

ln -f -s .../git/.classesOut/helloWorld.cls .../git/demo/force-app/main/default/classe
s/helloWorld.cls
```

## Testing
* Change the helloWorld class. Add blank space for example;
* Push the code to your scratch org

**Expected error**
> An object 'helloWorld' of type ApexClass was named in package.xml, but was not found in zipped directory

## License
[MIT](https://choosealicense.com/licenses/mit/)