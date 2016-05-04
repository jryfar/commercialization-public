---
author: joshbax-msft
title: Smart Card Minidriver Certification Test
description: Smart Card Minidriver Certification Test
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: f6d534d8-4df7-4afd-b4cf-f8627e4ea021
---

# Smart Card Minidriver Certification Test


This automated test verifies the operation of smart card minidrivers and associated smart cards.

Smart card minidrivers are software DLLs that are loaded by the Microsoft Base Cryptographic Service Provider (Base CSP) and the Microsoft Smart Card Key Storage Provider (SCKSP) to enable access to the functionality of the associated smart card. For smart card vendors, these minidrivers provide a simpler way to implement smart card functionality on Microsoft Windows operating systems than by developing a traditional Cryptographic Service Provider (CSP). (Historically smart card minidrivers were also referred to as smart card modules or smartcard card modules.)

This test performs functional, stress, performance, and reliability testing on a smart card minidriver. It calls the Microsoft BaseCSP and the Microsoft Smart Card Key Storage Provider and accesses the card minidriver methods directly, to test the correctness of operation of the card minidriver and the associated card. It also uses the Smart Card Resource Manager to access the card directly.

## Test details


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Associated requirements</strong></p></td>
<td><p>Device.Input.SmartCardMiniDriver.DoNotStopWhenResourcesAreUnavailable Device.Input.SmartCardMiniDriver.SpecsAndCertifications Device.Input.SmartCardMiniDriver.SupportMultipleInstancesOnASystem</p>
<p>[See the device hardware requirements.](http://go.microsoft.com/fwlink/p/?linkid=254483)</p></td>
</tr>
<tr class="even">
<td><p><strong>Platforms</strong></p></td>
<td><p>Windows 7 (x64) Windows 7 (x86) Windows RT (ARM-based) Windows 8 (x64) Windows 8 (x86) Windows Server 2012 (x64) Windows Server 2008 R2 (x64) Windows RT 8.1 Windows 8.1 x64 Windows 8.1 x86 Windows Server 2012 R2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Expected run time</strong></p></td>
<td><p>~180 minutes</p></td>
</tr>
<tr class="even">
<td><p><strong>Categories</strong></p></td>
<td><p>Certification Functional</p></td>
</tr>
<tr class="odd">
<td><p><strong>Type</strong></p></td>
<td><p>Automated</p></td>
</tr>
</tbody>
</table>

 

## Running the test


Before you run the test, complete the test setup as described in the test requirements: [Smart Card Reader Testing Prerequisites](smart-card-reader-testing-prerequisites.md).

In addition, this test requires the following hardware.

-   WHQL-certified smart card readers

## Troubleshooting


For troubleshooting information, see [Troubleshooting Device.Input Testing](troubleshooting-deviceinput-testing.md).

In addition, for smart card-specific troubleshooting information, see the following websites:

-   [Smart Card Minidriver Specification](http://go.microsoft.com/fwlink/p/?LinkId=233768)

-   [Smart Card Minidriver Certification Requirements](http://go.microsoft.com/fwlink/p/?LinkId=233769)

## More information


This test is located in Windows HCK Studio as a single job, although you can run the tool from outside the Windows HCK Studio environment also. Copy the binaries from the location that is specified below to a directory of your choice and you can run the test tool from there. You might also have to copy the Wttlog.dll shared library to your run directory. This DLL is installed with the Windows HCK client application.

Running the test tool outside the Windows HCK enables greater flexibility than running through the DTM because you can select tests individually. For more information, see the command line description later in this document.

If you are running the test from the Windows HCK, you need only one client computer. The Windows HCK documentation explains how to set up the Windows HCK controller, Windows HCK Studio, and Windows HCK client application on the test computer.

The following information applies to the computer that the test is being run on, regardless of whether you are running the tests inside or outside the Windows HCK environment.

To run the test, you must have your smart card minidriver installed on the computer and registered in the registry. (Also, for more information about certifying on a 64-bit version of the operating system, see the next section, "Certifying on a 64-bit version of the operating system"). And if your smart card device has the ISO 7816 ID-1 form factor, you must use a PC/SC compliant, WHQL-certified smart card reader for certification.

To run the tests, you must have two identical smart cards on the computer, and both must be in the prepared state, where prepared is defined by the Smart Card Minidriver Specification for Windows Base Cryptographic Service Provider (Base CSP) and Smart Card Key Storage Provider (KSP).

The tests that are run depend on the capabilities of the smart card minidriver. The capability of the smart card minidriver can be defined for the test tool in a specific configuration file that is named cmck\_config.xml, which you must put in the C:\\SmartCardMinidriverTest directory. The description of this file and a sample follow.

In a typical test run, if no configuration file is found, the test will ask you for confirmation to continue by using default values. For a logo submission however, you must supply a configuration file that matches the capability of your minidriver and card combination.

The default key values are as follows:

-   The default user pin is 0000.

-   The default administration key for challenge/response is all zeros.

**Certifying on a 64-bit version of the operating system**

When you certify on a 64-bit version of the operating system, you must also have the 32-bit version of your minidriver DLL installed on the system. You can put the DLL in the %systemroot%\\syswow64 subdirectory. You must put registry entries for the 32-bit version of the DLL under HKEY\_LOCAL\_MACHINE\\SOFTWARE\\wow6432Node\\Microsoft\\Cryptography\\Calais\\SmartCards.

``` syntax
```

When a 32-bit application that uses the minidriver is executed, it loads the 32-bit version of the minidriver.

**INF file for a smart card minidriver**

When you make a certification submission, you must supply an .inf file. Here is the sample .inf file for x86 architecture:

``` syntax
[Version]
Signature="$Windows NT$"
Class=SmartCard
ClassGuid={990A2BD7-E738-46c7-B26F-1CF8FB9F1391}
Provider=%ProviderName%
CatalogFile=delta.cat
DriverVer=06/21/2006,6.1.6473.1 

[DefaultInstall]
CopyFiles=System32_CopyFiles
AddReg=MiniDriver_AddReg

[Manufacturer]
%ProviderName%=CompanyName,NTx86,NTx86.6.1 

[CompanyName.NTx86]
%ScSuperCardDeviceName%=ScSuperCard_Install,SCFILTER\CID_51FF0800 

[CompanyName.NTx86.6.1]
%ScSuperCardDeviceName%=ScSuperCard61_Install,SCFILTER\CID_51FF0800 

[SourceDisksFiles]
supercm.dll=1

[SourceDisksNames]
1 = %MediaDescription% 

[ScSuperCard_Install.NT]
CopyFiles=System32_CopyFiles
AddReg=MiniDriver_AddReg 

[ScSuperCard61_Install.NT]
CopyFiles=System32_CopyFiles
AddReg=MiniDriver_AddReg 

[SCSuperCard61_Install.NT.Services]
Include=umpass.infNeeds=UmPass.Services

[UMPassService_Install]
DisplayName=%umpass.SVCDESC%

; Friendly Name of the Service
ServiceType    = 1                    ;SERVICE_KERNEL_DRIVER
StartType      = 3                    ;SERVICE_DEMAND_START
ErrorControl   = 1                    ;SERVICE_ERROR_NORMAL
ServiceBinary  = %12%\umpass.sys
LoadOrderGroup = Extended Base

[System32_CopyFiles]
supercm.dll

[MiniDriver_AddReg]
HKLM,%SmartCardName%,"ATR",0x00000001,3b,04,51,ff,08,00
HKLM,%SmartCardName%,"ATRMask",0x00000001,ff,ff,ff,ff,ff,ff
HKLM,%SmartCardName%,"Crypto Provider",0x00000000,"Microsoft Base Smart Card Crypto Provider"
HKLM,%SmartCardName%,"Smart Card Key Storage Provider",0x00000000,"Microsoft Smart Card Key Storage Provider"
HKLM,%SmartCardName%,"80000001",0x00000000,%SmartCardCardModule%

[DestinationDirs]
System32_CopyFiles=10,system32

[FriendlyName]
ScFriendlyName="Super Card"
; =================== Generic ==================================

[Strings]
ProviderName="ACME"
MediaDescription="Super Card Mini Driver Installation Disk"
SCSuperCardDeviceName="Super Card Mini-driver"
SmartCardName="SOFTWARE\Microsoft\Cryptography\Calais\SmartCards\Super Card"
SmartCardCardModule="supercm.dll"
umpass.SVCDESC = "Microsoft UMPass Driver" Inf file sample for x64 architecture: [Version]
Signature="$Windows NT$"
Class=SmartCard
ClassGuid={990A2BD7-E738-46c7-B26F-1CF8FB9F1391}
Provider=%ProviderName%
CatalogFile=delta.cat
DriverVer=06/21/2006,6.1.6473.1 

[DefaultInstall]
CopyFiles=System32_CopyFiles
CopyFiles=Syswow64_CopyFiles
AddReg=MiniDriver_AddReg 

[Manufacturer]
%ProviderName%=CompanyName,NTamd64,NTamd64.6.1 

[CompanyName.NTamd64]
%ScSuperCardDeviceName%=ScSuperCard_Install,SCFILTER\CID_51FF0800 

[CompanyName.NTamd64.6.1]
%ScSuperCardDeviceName%=ScSuperCard61_Install,SCFILTER\CID_51FF0800 

[SourceDisksFiles]
supercm64.dll=1
supercm.dll=1

[SourceDisksNames]
1 = %MediaDescription% 

[ScSuperCard_Install.NT]
CopyFiles=System32_CopyFiles
CopyFiles=Syswow64_CopyFiles
AddReg=MiniDriver_AddReg 

[ScSuperCard61_Install.NT]
CopyFiles=System32_CopyFiles
CopyFiles=Syswow64_CopyFiles
AddReg=MiniDriver_AddReg 

[SCSuperCard61_Install.NT.Services]
Include=umpass.inf
Needs=UmPass.Services

[UMPassService_Install]
DisplayName    = %umpass.SVCDESC%

; Friendly Name of the Service
ServiceType    = 1                    ;SERVICE_KERNEL_DRIVER
StartType      = 3                    ;SERVICE_DEMAND_START
ErrorControl   = 1                    ;SERVICE_ERROR_NORMAL
ServiceBinary  = %12%\umpass.sys
LoadOrderGroup = Extended Base

[System32_CopyFiles]
supercm.dll,supercm64.dll

[Syswow64_CopyFiles]
supercm.dll

[MiniDriver_AddReg]
HKLM,%SmartCardName%,"ATR",0x00000001,3b,04,51,ff,08,00
HKLM,%SmartCardName%,"ATRMask",0x00000001,ff,ff,ff,ff,ff,ff
HKLM,%SmartCardName%,"Crypto Provider",0x00000000,"Microsoft Base Smart Card Crypto Provider"
HKLM, %SmartCardName%,"Smart Card Key Storage Provider",0x00000000,"Microsoft Smart Card Key Storage Provider"
HKLM,%SmartCardName%,"80000001",0x00000000,%SmartCardCardModule%
HKLM,%SmartCardNameWOW64%,"ATR",0x00000001,3b,04,51,ff,08,00
HKLM,%SmartCardNameWOW64%,"ATRMask",0x00000001,ff,ff,ff,ff,ff,ff
HKLM,%SmartCardNameWOW64%,"Crypto Provider",0x00000000,"Microsoft Base Smart Card Crypto Provider"
HKLM,%SmartCardNameWOW64%,"Smart Card Key Storage Provider",0x00000000,"Microsoft Smart Card Key Storage Provider"
HKLM,%SmartCardNameWOW64%,"80000001",0x00000000,%SmartCardCardModule% 

[DestinationDirs]
System32_CopyFiles=10,system32
Syswow64_CopyFiles=10,syswow64 

[FriendlyName]
ScFriendlyName="Super Card" 
; =================== Generic ==================================

[Strings]
ProviderName="ACME"
MediaDescription="Super Card Mini Driver Installation Disk"
SCSuperCardDeviceName="Super Card Mini-driver"
SmartCardName="SOFTWARE\Microsoft\Cryptography\Calais\SmartCards\Super Card"
SmartCardNameWOW64="SOFTWARE\Wow6432Node\Microsoft\Cryptography\Calais\SmartCards\Microsoft Virtual Card"
SmartCardCardModule="supercm.dll"
SmartCardCardModule64="supercm64.dll"
umpass.SVCDESC = "Microsoft UMPass Driver"
```

**Smart Card Minidriver Configuration File Description (cmck\_config.xml)**

**CMCK\_config.xml consists of below sections for V5, V6, and V7, depending on the versions V5/V6/V7 your minidriver supported. Please note: V5/V6/V7 keywords are uppercased.**

``` syntax
<CMCKConfig>
  <V5>...</V5>
  <V6>...</V6>
  <V7>...</V7>
</CMCKConfig>
```

**Each section is filled up with the XML tags described below. Please also note each section may have different settings which depends on what the minidriver supports for that versions.**

You can set the following values in each section of the configuration file (in the structure as shown):

-   &lt;Version&gt; is a mandatory field that defines the configuration file's version number. Currently, the expected version is "2" of the CMCK XML configuration file for V6 and V7 sections, and "1" for V5 section.

-   &lt;CardDefaults&gt; contains parameters that are used both for the certification run and for regular test runs.

    -   &lt;DefaultPins&gt; - pin values that must be used for authentication-related functions

        &lt;PinEntry&gt;&lt;RoleID&gt; - allowed value: 1 - 7

        &lt;PinEntry&gt;&lt;Type&gt; - allowed value: "AlphanumericPinType", "ChallengeResponsePinType", "EmptyPinType", "ExternalPinType"

        &lt;PinEntry&gt;&lt;Value&gt; - list of space-separated hexadecimal bytes of the pin. Default value: "0x30 0x30 0x30 0x30" (that is "0000")

        &lt;PinEntry&gt;&lt;Blocking&gt; \[BOOLEAN\] - states if the card supports blocking the card for this kind of user when the wrong pin is presented too many times. Default "True".

        &lt;PinEntry&gt;&lt;Linking&gt; \[BOOLEAN\] - states if this pin is linked to another pin, i.e changing one pin will make the other one changed. Default "False".

        &lt;PinEntry&gt;&lt;AllowZeroLength&gt; \[BOOLEAN\] - states if the card allows the pin to be empty. Default "False".

-   &lt;CardSupports&gt; -defines optional features that are supported by the card or card minidriver, such as algorithms, key types, and so on. The test will cover the supported features.

    1.  &lt;MinimumVersion&gt; \[DWORD\] contains the minimum version of the context CARD\_DATA structure that the card minidriver supports. For more information, see the Smart Card Minidriver specification. The allowed value is "4," "5," "6," "7".

    2.  &lt;CurrentVersion&gt; \[DWORD\] contains the version of the context CARD\_DATA structure for which the card minidriver was designed. For more information, see the Smart Card Minidriver specification. The allowed value is "5," "6," "7".

    3.  &lt;LoadingUnderCAPI&gt; \[BOOLEAN\] is "True" if the card minidriver supports being loaded under CAPI and is "False" otherwise.

    4.  &lt;LoadingUnderCNG&gt; \[BOOLEAN\] is "True" if the card minidriver supports being loaded under CNG and is "False" otherwise. Note that at least one of the two attributes must be "True" (CAPI or CNG).

    5.  &lt;KeyImport&gt; \[BOOLEAN\] is "True" if the card minidriver supports key import and is "False" otherwise.

    6.  &lt;KeyTypes&gt; contains space-separated list of key types that the card minidriver supports. The allowed values are "AT\_ECDH\_P256", "AT\_ECDH\_P384", "AT\_ECDH\_P521", "AT\_ECDSA\_P256", "AT\_ECDSA\_P384", or "AT\_ECDSA\_P521" for ECC keys and "AT\_SIGNATURE" or "AT\_KEYEXCHANGE" for RSA keys. &lt;OnCardPadding&gt; \[BOOLEAN\] is "True" if on-card padding is supported and is "False" otherwise.

    7.  &lt;PaddingAlgorithms&gt; contains a space-separated list of supported on-card padding algorithms (only valid when &lt;OnCardPadding&gt; is "True"). The allowed values are "CARD\_PADDING\_NONE", "CARD\_PADDING\_PKCS1", and "CARD\_PADDING\_PSS".

    8.  &lt;SignHashAlgorithms&gt; contains a space-separated list of hash algorithms that the card minidriver supports for signing (aiHashAlg in the CARD\_SIGNING\_INFO structure). Allowed values are "CALG\_MD2", "CALG\_MD4", "CALG\_MD5", "CALG\_SHA", "CALG\_SHA1", "CALG\_SHA\_256", CALG\_SHA\_384" and CALG\_SHA\_512". For more details, see the Smart Card Minidriver specification.

    9.  &lt;SignHashFlags&gt; contains a space-separated list of CryptSignHash flags that the card minidriver supports. The allowed values are "CRYPT\_NOHASHOID" and "CRYPT\_X931\_FORMAT".

    10. &lt;SignReturnBufferSize&gt; \[BOOLEAN\] is "True" when the card minidriver supports the CARD\_BUFFER\_SIZE\_ONLY value for dwSigningFlags in the CARD\_SIGNING\_INFO structure. For more information, see the Smart Card Minidriver specification.

    11. &lt;KDFTypes&gt; contains a space-separated list of key derive functions that the card minidriver supports. The allowed values are "HASH", "HMAC", "TLS\_PRF", and "SP800\_56A\_CONCAT". For more information, see the Smart Card Minidriver specification.

    12. &lt;KDFHashAlgorithms&gt; contains a space-separated list of hash algorithms that the card minidriver supports for the key derivation functions (KDF). The Smart Card Minidriver specification contains more details. The allowed values are defined in the bcrypt.h header file and are the values for BCRYPT\_XXXX\_ALGORITHM defines (for example, "SHA256" (BCRYPT\_SHA256\_ALGORITHM) or "MD5" (BCRYPT\_MD5\_ALGORITHM)).

    13. &lt;Uses2Key3DES&gt; \[BOOLEAN\] is "True" if the card minidriver ONLY supports 2 key 3DES. When this value is "False", it indicates the card minidriver use default 3 key 3DES. Default value is "False".

    14. &lt;KDFHMACFlag&gt; \[BOOLEAN\] is "True" if the card minidriver supports the BCRYPT\_KDF\_HMAC key derive function that has the KDF\_USE\_SECRET\_AS\_HMAC\_KEY\_FLAG flag (see the Crypto Next Generation (CNG) documentation available on MSDN CNG for more information). When this value is "False", the test will provide an explicit HMAC key and will not set the KDF\_USE\_SECRET\_AS\_HMAC\_KEY\_FLAG flag for affected tests.

    15. &lt;ChallengePadding&gt;\[BOOLEAN\] is "True" if the return of card challenge data contains a padding bit. Default value is "False".

    16. &lt;SupportsCardGetChallenge&gt; \[BOOLEAN\] - "True" if CM supports CardGetChallenge API (this is used for read only cards).

    17. &lt;SupportsCardAuthenticateChallenge&gt; \[BOOLEAN\] - "True" if CM supports CardAuthenticateChallenge API (this is used for read only cards).

    18. &lt;SupportsCardGetChallengeEx&gt; \[BOOLEAN\] - "True" if CM supports CardGetChallengeEx API (this is used for read only cards).

    19. &lt;SupportsCardUnblockPin&gt; \[BOOLEAN\] - "True" if CM supports CardUnblockPin API (this is used for read only cards).

    20. &lt;SupportsCardChangeAuthenticator&gt; \[BOOLEAN\] - "True" if CM supports CardChangeAuthenticator API (this is used for read only cards).

    21. &lt;SupportsCardChangeAuthenticatorEx&gt; \[BOOLEAN\] - "True" if CM supports CardChangeAuthenticatorEx API (this is used for read only cards).

-   &lt;TestSuiteDefaults&gt; contains parameters that influence the test runs only. (The certification run uses its own default values.)

    1.  &lt;INF&gt; contains information about INF file that is used in smartcard PnP.

        -   &lt;INFFile&gt; contains the location and file name for the smartcard PnP INF file, which matches the smartcard/minidriver to be tested. This is mandatory for all minidrivers other than in-box PIV/GICS class minidriver.

    2.  &lt;Logging&gt; contains logging options that apply to the test runs

        1.  &lt;LogFile&gt; contains the location and file name for the XML log file. The default is "CMCK\_log.xml" (which means that the file will be created in the current directory). Note that the log file will be overwritten.

        2.  &lt;LogToConsole&gt; \[BOOLEAN\] is "True" when the logs will be displayed on the console along with writing to the log file. The default value is "True".

        3.  &lt;CertifyLogLevel&gt; specifies the log level (0, 1, or 2). 0 means minimal logging, 1 logs function entry and exit, and 2 means full logging. Full logging is always used when you run tests individually and is mainly to help with development. When you are making a logo submission, we recommend setting the log level to 0.

**Configuration File Structure**

The following code example shows a sample configure file.

``` syntax

<CMCKConfig>
  <V6>
    <Version>2</Version>
    <CardDefaults>
      <DefaultPins>
        <PinEntry>
          <RoleID>1</RoleID>
          <Type>AlphaNumericPinType</Type>
          <Value>0x30 0x30 0x30 0x30</Value>
          <Blocking>True</Blocking>
          <AllowZeroLength>False</AllowZeroLength>
        </PinEntry>
        <PinEntry> 
          <RoleID>2</RoleID>
          <Type>ChallengeResponsePinType</Type>
          <Value>0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00</Value>
          <Blocking>True</Blocking>
          <AllowZeroLength>False</AllowZeroLength>
        </PinEntry>
        <PinEntry>
          <RoleID>3</RoleID>
          <Type>AlphaNumericPinType</Type>
          <Value>0x30 0x30 0x30 0x30</Value>
          <Blocking>True</Blocking>
          <AllowZeroLength>False</AllowZeroLength>
        </PinEntry>
        <PinEntry>
         <RoleID>4</RoleID>
         <Type>AlphaNumericPinType</Type>
         <Value>0x30 0x30 0x30 0x30</Value>
         <Blocking>True</Blocking>
         <AllowZeroLength>False</AllowZeroLength>
       </PinEntry>
       <PinEntry>
          <RoleID>5</RoleID>
          <Type>AlphaNumericPinType</Type>
          <Value>0x30 0x30 0x30 0x30</Value>
          <Blocking>True</Blocking>
          <AllowZeroLength>False</AllowZeroLength>
       </PinEntry>
       <PinEntry>
         <RoleID>6</RoleID>
         <Type>AlphaNumericPinType</Type>
         <Value>0x30 0x30 0x30 0x30</Value>
         <Blocking>True</Blocking>
         <AllowZeroLength>False</AllowZeroLength>
        </PinEntry>
        <PinEntry>
          <RoleID>7</RoleID>
          <Type>AlphaNumericPinType</Type>
          <Value>0x30 0x30 0x30 0x30</Value>
          <Blocking>True</Blocking>
          <AllowZeroLength>False</AllowZeroLength>
        </PinEntry>
      </DefaultPins>
      <CardSupports>
        <MinimumVersion>4</MinimumVersion>
        <CurrentVersion>6</CurrentVersion>
        <LoadingUnderCAPI>True</LoadingUnderCAPI>
        <LoadingUnderCNG>True</LoadingUnderCNG>
        <KeyImport>True</KeyImport>
        <KeyTypes>AT_SIGNATURE AT_KEYEXCHANGE</KeyTypes>
        <OnCardPadding>False</OnCardPadding>
        <PaddingAlgorithms>CARD_PADDING_PKCS1</PaddingAlgorithms>
        <SignHashAlgorithms>CALG_MD5 CALG_SHA CALG_SHA1 CALG_SHA_256 CALG_SHA_384 CALG_SHA_512</SignHashAlgorithms>
        <SignHashFlags />
        <SignReturnBufferSize>True</SignReturnBufferSize>
        <KDFTypes>HASH</KDFTypes>
        <KDFHashAlgorithms>SHA1 SHA256 SHA384 SHA512</KDFHashAlgorithms>
        <KDFHMACflag>False</KDFHMACflag>
        <SupportsCardGetChallenge>True</SupportsCardGetChallenge>
        <SupportsCardAuthenticateChallenge>True</SupportsCardAuthenticateChallenge>
        <SupportsCardGetChallengeEx>True</SupportsCardGetChallengeEx>
        <SupportsCardUnblockPin>True</SupportsCardUnblockPin>
        <SupportsCardChangeAuthenticator>True</SupportsCardChangeAuthenticator>
        <SupportsCardChangeAuthenticatorEx>True</SupportsCardChangeAuthenticatorEx>
      </CardSupports>
    </CardDefaults>
    <TestSuiteDefaults>
      <INF>
        <INFFile>C:\SmartcardMinidriverTest\minidriver.inf</INFFile>
      </INF>
      <Logging>
        <LogFile>CMCK_log.xml</LogFile>
        <LogToConsole>True</LogToConsole>
      </Logging>
      <TestParams>
        <TestParam>
          <Test>MultiThreaded</Test>
          <Name>t</Name>
          <Value>5</Value>
        </TestParam>
        <TestParam>
          <Test>MultiThreaded</Test>
          <Name>n</Name>
          <Value>5</Value>
        </TestParam>
        <TestParam>
          <Test>NonRepeatingChallenge</Test>
          <Name>n</Name>
          <Value>300</Value>
        </TestParam>
      </TestParams>
    </TestSuiteDefaults>
  </V6>
  <V5>
    <Version>1</Version>
      <CardDefaults>
        <DefaultPins>
          <PinEntry>
            <Type>User</Type>
            <Value>0x30 0x30 0x30 0x30</Value>
            <Blocking>True</Blocking>
            <AllowZeroLength>False</AllowZeroLength>
          </PinEntry>
        </DefaultPins>
        <DefaultKeys>
          <KeyEntry>
            <Type>Admin</Type>
            <Algorithm>3DES</Algorithm>
            <Mode>ECB</Mode>
            <Blocking>True</Blocking>
            <Value>0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00</Value>        </KeyEntry>
        </DefaultKeys>
        <CardSupports>
          <MinimumVersion>4</MinimumVersion>
          <CurrentVersion>6</CurrentVersion>
          <LoadingUnderCAPI>True</LoadingUnderCAPI>
          <LoadingUnderCNG>True</LoadingUnderCNG>
          <KeyImport>True</KeyImport>
          <KeyTypes>AT_SIGNATURE AT_KEYEXCHANGE</KeyTypes>
          <OnCardPadding>False</OnCardPadding>
          <PaddingAlgorithms>CARD_PADDING_NONE</PaddingAlgorithms>
          <SignHashAlgorithms>CALG_MD5 CALG_SHA CALG_SHA1</SignHashAlgorithms>
          <SignHashFlags>CRYPT_NOHASHOID</SignHashFlags>
          <SignReturnBufferSize>False</SignReturnBufferSize>
          <KDFTypes>HASH</KDFTypes>
          <KDFHashAlgorithms>SHA1 SHA256 SHA384 SHA512</KDFHashAlgorithms>
          <KDFHMACflag>False</KDFHMACflag>
      </CardSupports>
    </CardDefaults>
    <TestSuiteDefaults>
      <INF>
        <INFFile>C:\SmartcardMinidriverTest\minidriver.inf</INFFile>
      </INF>
      <Logging>
        <LogFile>CMCK_log.xml</LogFile>
        <LogToConsole>True</LogToConsole>
      </Logging>
      <TestParams>
        <TestParam>
          <Test>MultiThreaded</Test>
          <Name>t</Name>
          <Value>5</Value>
        </TestParam>
        <TestParam>
          <Test>MultiThreaded</Test>
            <Name>n</Name>
            <Value>5</Value>
        </TestParam>
        <TestParam>
          <Test>NonRepeatingChallenge</Test>
            <Name>n</Name>
            <Value>300</Value>
        </TestParam>
      </TestParams>
    </TestSuiteDefaults>
  </V5>
</CMCKConfig>
```

### Command syntax

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Command option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>cmck &lt;command&gt; [options]</strong></p></td>
<td><p>Runs the test.</p></td>
</tr>
</tbody>
</table>

 

**Note**  
For command-line help for this test binary, type `cmck help [command]`

 

### File list

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>File</th>
<th>Location</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>cmck.exe</p></td>
<td><p><em>&lt;[testbinroot]&gt;</em>\nttest\dstest\security\core\bin\credentials\smartcard</p></td>
</tr>
<tr class="even">
<td><p>cmck_simuse.exe</p></td>
<td><p><em>&lt;[testbinroot]&gt;</em>\nttest\dstest\security\core\bin\credentials\smartcard</p></td>
</tr>
<tr class="odd">
<td><p>intrcptr.dll</p></td>
<td><p><em>&lt;[testbinroot]&gt;</em>\nttest\dstest\security\core\bin\credentials\smartcard</p></td>
</tr>
</tbody>
</table>

 

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bp_hck\p_hck%5D:%20Smart%20Card%20Minidriver%20Certification%20Test%20%20RELEASE:%20%284/27/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



