<h1>cgic 2.08: an ANSI C library for CGI Programming</h1>
<h2>By <a href="http://boutell.dev">Thomas Boutell</a></h2>
<blockquote>
<strong>IMPORTANT NOTICES:</strong>
<p>
If you have CGIC 1.05 or earlier, you should upgrade to CGIC 1.07,
or to CGIC 2.02 or better, in order to obtain important security fixes.
<p>
If you have CGIC 2.0 or CGIC 2.01 and you use the cgiCookie routines, 
you should upgrade to CGIC 2.02 or better, in order to obtain
important security fixes.
</blockquote>
<h3>Table of Contents</h3>
<ul>
<li><a href="#credits">Credits and license terms</a>
<li><a href="#whatsnew208">What's new in version XYZ of CGIC?</a>
<li><a href="#whatis">What is cgic?</a>
<li><a href="#obtain">Obtaining cgic</a>
<li><a href="#build">Building and testing cgic: a sample application</a>
<li><a href="#nocompile">What to do if it won't compile</a>
<li><a href="#howto">How to write a cgic application</a>
<li><a href="#images">How can I generate images from my cgic application?</a>
<li><a href="#debug">CGI debugging features: using capture</a>
<li><a href="#functions">cgic function reference</a>
<li><a href="#variables">cgic variable reference</a>
<li><a href="#resultcodes">cgic result code reference</a>
<li><a href="#index">cgic quick index</a>
</ul>

<h3><a name="credits">Credits and License Terms</a></h3>

<p>
cgic is now distributed under the MIT license:
</p>
<p>
Copyright (c) 1996-2020 Thomas Boutell
</p>
<p>
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
</p>
<p>
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
</p>
<p>
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
</p>
<p>
Thanks are due to Robert Gustavsson, Ken Holervich, Bob Nestor, 
Jon Ribbens, Thomas Strangert, Wu Yongwei, and other CGIC users 
who have corresponded over the years. Although the implementation
of multipart/form-data file upload support in CGIC 2.x is my own, 
I particularly wish to thank those who submitted their own 
implementations of this feature.
</p>
<h3><a name="whatsnew208">What's new in version 2.08?</a></h3>
Beginning with version 2.08, `cgic` offers more options when setting cookies. To enhance security, the following options can be used when setting a cookie:
<ul>
<li>Secure</li>
<li>HttpOnly</li>
<li>SameSite=Strict</li>
</ul>
The new function
<code><a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a></code>
allows you to set these options. The function
<code><a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a></code>
is kept for compatibility reasons.
<h3><a name="whatsnew207">What's new in version 2.07?</a></h3>
Per a suggestion by Geoff Mulligan, cgic now tolerates keys without a value in URL-encoded GET and POST submissions. Although the HTML5 spec discourages it, there are existing systems in the wild that do leave the `=` off entirely if the value is an empty string. Beginning with version 2.07, `cgic` handles this as you'd expect: you get an entry with the corresponding key and an empty string as the value. A simple unit test compilation target was also added, to test this feature and rule out side effects.
<h3><a name="whatsnew206">What's new in version 2.06?</a></h3>
A long list of significant fixes generously contributed by Jeffrey Hutzelman.
<a href="https://github.com/boutell/cgic/commit/2065a2dfa480209bd6171acb2a4edd6e1c27a062"> These are especially important on platforms where attempting to read beyond the content length stated by the request can lead to a deadlock. Please see the commit notes.</a>
<h3><a name="whatsnew205">What's new in version 2.05?</a></h3>
Uploaded files properly closed; corrects a resource leak and enables
file uploads to work properly on platforms with particular file
locking semantics.
<h3><a name="whatsnew204">What's new in version 2.04?</a></h3>
Documentation fixes: the cgiHtmlEscape, cgiHtmlEscapeData,
cgiValueEscape, and cgiValueEscapeData routines were named
incorrectly in the manual. No code changes in version 2.04.
<h3><a name="whatsnew203">What's new in version 2.03?</a></h3>
<ul>
<li>Support for setting cookies has been reimplemented. The new
code closely follows the actual practice of web sites that successfully
use cookies, rather than attempting to implement the specification.
The new code can successfully set more than one cookie at a time in
typical web browsers.
</ul>
<h3><a name="whatsnew202">What's new in version 2.02?</a></h3>
<ul>
<li>In CGIC 2.0 and 2.01, if the HTTP_COOKIE environment variable
was exactly equal to the name of a cookie requested with cgiCookieString,
with no value or equal sign or other characters present, a buffer
overrun could take place. This was not normal behavior and it is
unknown whether any actual web server would allow it to occur, however
we have of course released a patch to correct it. 
Thanks to Nicolas Tomadakis.
<li>cgiCookieString returned cgiFormTruncated when cgiFormSuccess would
be appropriate. Fixed; thanks to Mathieu Villeneuve-Belair.
<li>Cookies are now set using a simpler Set-Cookie: header, and with
one header line per cookie, based on data collected by Chunfu Lai. 
<li>Memory leaks in cgiReadEnvironment fixed by Merezko Oleg. These
memory leaks were <em>not</em> experienced in a normal CGI situation, only
when reading a saved CGI environment.
</ul>
<h3><a name="whatsnew201">What's new in version 2.01?</a></h3>
<ul>
<li>Makefile supports "make install"
<li>Compiles without warnings under both C and C++ with strict
warnings and strict ANSI compliance enabled
<li>Builds out of the box on Windows (#include &lt;fcntl.h&gt; was needed)
<li>Rare problem in cgiReadEnvironment corrected; no impact on
normal CGI operations
<li>cgiCookieString now sets the result to an empty string
when returning cgiFormNotFound
<li>Minor code cleanups
</ul>
<h3><a name="whatsnew200">What's new in version 2.0?</a></h3>
1. CGIC 2.0 provides support for file upload fields. User-uploaded
files are kept in temporary files, to avoid the use of
excessive swap space (Solaris users may wish to change the
<code>cgicTempDir</code> macro in cgic.c before compiling).
The <code><a href="#cgiFormFileName">cgiFormFileName</a></code>, 
<code><a href="#cgiFormFileContentType">cgiFormFileContentType</a></code>, 
<code><a href="#cgiFormFileSize">cgiFormFileSize</a></code>, 
<code><a href="#cgiFormFileOpen">cgiFormFileOpen</a></code>, 
<code><a href="#cgiFormFileRead">cgiFormFileRead</a></code>, and
<code><a href="#cgiFormFileClose">cgiFormFileClose</a></code> functions
provide a complete interface to this new functionality. Remember,
the <code>enctype</code> attribute of the <code>form</code> tag
must be set to <code>multipart/form-data</code> when
<code>&lt;input type="file"&gt;</code> tags are used.
<p>
2. CGIC 2.0 provides support for setting and examining cookies
(persistent data storage on the browser side).
The <code><a href="#cgiCookieString">cgiCookieString</a></code>,
and <code><a href="#cgiCookieInteger">cgiCookieInteger</a></code>
and <code><a href="#cgiCookies">cgiCookies</a></code>
functions retrieve cookies. The 
<code><a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a></code>
and <code><a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a></code> functions set cookies.
<p>
3. CGIC 2.0 offers a convenient way to retrieve a list of all form fields.
The new <code><a href="#cgiFormEntries">cgiFormEntries</a></code>
function performs this operation.
<p>
4. CGIC 2.0 provides convenience functions to correctly escape
text before outputting it as part of HTML, or as part of the 
value of a tag attribute, such as the <code>HREF</code> or
<code>VALUE</code> attribute. See 
<code><a href="#cgiHtmlEscape">cgiHtmlEscape</a></code>,
<code><a href="#cgiHtmlEscapeData">cgiHtmlEscapeData</a></code>,
<code><a href="#cgiValueEscape">cgiValueEscape</a></code> and
<code><a href="#cgiValueEscapeData">cgiValueEscapeData</a></code>.
<p>
5. Users have often asked the correct way to determine which submit
button was clicked. This could always be accomplished in previous versions,
but CGIC 2.0 also provides 
<a href="#cgiFormSubmitClicked">cgiFormSubmitClicked</a>,
a convenient alternate label for the 
<a href="#cgiFormCheckboxSingle">cgiFormCheckboxSingle</a> function.
<h3><a name="whatsnew107">What's new in version 1.07?</a></h3>
A problem with the cgiFormString and related functions has been
corrected. These functions were previously incorrectly returning cgiFormTruncated
in cases where the returned string fit the buffer exactly.
<h3><a name="whatsnew106">What's new in version 1.06?</a></h3>
1. A potentially significant buffer overflow problem has been
corrected. Jon Ribbens correctly pointed out to me (and to the
Internet's bugtraq mailing list) that the cgiFormEntryString
function, which is used directly or indirectly by almost all
CGIC programs, can potentially write past the buffer passed
to it by the programmer. This bug has been corrected.
Upgrading to version 1.06 is <strong>strongly recommended.</strong>
<P>
2. The function <code>cgiSaferSystem()</code> has been
removed entirely. This function escaped only a few metacharacters,
while most shells have many, and there was no way to account for
the many different operating system shells that might be in use
on different operating systems. Since this led to a false sense
of security, the function has been removed. It is our recommendation
that user input should never be passed directly on the command line
unless it has been carefully shown to contain only characters
regarded as safe and appropriate by the programmer. Even then, it is
better to design your utilities to accept their input from standard
input rather than the command line.
<h3><a name="whatsnew105">What's new in version 1.05?</a></h3>
Non-exclusive commercial license fee reduced to $200.
<h3><a name="whatsnew104">What's new in version 1.04?</a></h3>
For consistency with other packages, the standard Makefile
now produces a true library for cgic (libcgic.a). 
<h3><a name="whatsnew103">What's new in version 1.03?</a></h3>
Version 1.03 sends line feeds only (ascii 10) to end 
Content-type:, Status:, and other HTTP protocol output lines,
instead of CR/LF sequences. The standard specifies CR/LF.
Unfortunately, too many servers reject CR/LF to make
implementation of that standard practical. No server
tested ever rejects LF alone in this context. 
<h3><a name="whatsnew102">What's new in version 1.02?</a></h3>
Version 1.02 corrects bugs in previous versions:
<ul>
<li>
<a href="#cgiFormDoubleBounded">cgiFormDoubleBounded</a> specified
its arguments in the wrong order, with surprising results.
This bug has been corrected.
<li>
Many small changes have been made to increase compatibility.
cgic now compiles with no warnings under the compilers
available at boutell.dev.
</ul>
<h3><a name="whatsnew101">What's new in version 1.01?</a></h3>
Version 1.01 adds no major functionality but corrects 
significant bugs and incompatibilities:
<ul>
<li>
<a href="#cgiFormInteger">cgiFormInteger</a>,
<a href="#cgiFormIntegerBounded">cgiFormIntegerBounded</a>,
<a href="#cgiFormDouble">cgiFormDouble</a> and
<a href="#cgiFormDoubleBounded">cgiFormDoubleBounded</a> now
accept negative numbers properly. They also accept positive
numbers with an explicit + sign.
<li>Hex values containing the digit <code>9</code> are
now properly decoded.
<li><a href="#cgiFormString">cgiFormString</a> now
represents each newline as a single line feed (ascii 10 decimal)
as described in the documentation, not a carriage return
(ascii 13 decimal) as in version 1.0. The latter approach
pleased no one.
<li><a href="#cgiFormString">cgiFormString</a> and
<a href="#cgiFormStringNoNewlines">cgiFormStringNoNewlines</a>
no longer erroneously return cgiFormEmpty in place of
cgiFormSuccess.
<li>The main() function of cgic now flushes standard output
and sleeps for one second before exiting in order to inhibit
problems with the completion of I/O on some platforms. This was
not a cgic bug per se, but has been reported as a common problem
with CGI when used with the CERN server. This change should
improve compatibility.
<li>The single selection example in the testform.html
example now works properly. This was an error in the
form itself, not cgic.
<li><a href="#cgiRemoteUser">cgiRemoteUser</a> and
<a href="#cgiRemoteIdent">cgiRemoteIdent</a> are now
documented accurately. They were reversed earlier.
</ul>
<h3><a name="whatis">What is cgic?</a></h3>
cgic is an ANSI C-language library for the creation of CGI-based
World Wide Web applications. For basic information about
the CGI standard, see the <a href="http://hoohoo.ncsa.uiuc.edu/cgi/">
CGI documentation</a> at NCSA.
<p>
cgic performs the following tasks:
<ul>
<li>Parses form data, correcting for defective and/or inconsistent browsers
<li>Transparently accepts both GET and POST form data
<li>Accepts uploaded files as well as regular form fields
<li>Provides functions to set and retrieve "cookies"
(browser-side persistent information)
<li>Handles line breaks in form fields in a consistent manner
<li>Provides string, integer, floating-point, and single- and
multiple-choice functions to retrieve form data
<li>Provides bounds checking for numeric fields
<li>Loads CGI environment variables into C strings which are always non-null
<li>Provides a way to capture CGI situations for replay in a debugging
environment, including file uploads and cookies
</ul>
<p>
cgic is compatible with any CGI-compliant server environment, and
compiles without modification in Posix/Unix/Linux and Windows
environments.
<h3><a name="obtain">Obtaining cgic</a></h3>
cgic is distributed via the web in two forms: as a Windows-compatible
.ZIP file, and as a gzipped tar file. Most users of Windows and
related operating systems have access to 'unzip' or 'pkunzip'. All modern Unix 
systems come with 'gunzip' and 'tar' as standard equipment, and gzip/gunzip
is not difficult to find if yours does not. Versions
of these programs for other operating systems are widely
available if you do not already have them.
<p>
<strong>Important:</strong> to use cgic, you will need an ANSI-standard
C compiler. Under Unix, just obtain and use gcc. Most Unix systems have
standardized on gcc. Users of Windows operating systems should not have
ANSI C-related problems as all of the popular compilers follow the ANSI 
standard.
<p>
<strong>Note for Windows Programmers:</strong> you must use a modern
32-bit compiler. Visual C++ 2.0 or higher, Borland C++ and the
mingw32 gcc compiler are all appropriate, as is cygwin. Do 
<strong>NOT</strong> use an ancient 16-bit DOS executable compiler, please.
<blockquote>
<h4>What Operating System Does Your WEB SERVER Run?</h4>
Remember, the computer on your desk is usually NOT your web server.
Compiling a Windows console executable will not give you a CGI program that
can be installed on a Linux-based server. 
</blockquote>
Your web browser should inquire whether to save the file to disk
when you select one of the links below. Under Unix and compatible
operating systems, save it, then issue the following
commands to unpack it:
<pre>
gunzip cgic207.tar.gz
tar -xf cgic207.tar
</pre>
This should produce the subdirectory 'cgic207', which will contain
the complete cgic distribution for version 2.07, including a copy of this 
documentation in the file cgic.html.
<p>
Under Windows and compatible operating systems, save it,
open a command prompt window, and issue the following commands to unpack it:
<pre>
unzip /d cgic207.zip
</pre>
Or use the unzip utility of your choice.
<p>
This command also produces the subdirectory 'cgic207', which will contain
the complete cgic distribution, including a copy of this 
documentation in the file cgic.html.
<p>
cgic is available via the web from www.boutell.dev:
<ul>
<li><a href="http://www.boutell.dev/cgic/cgic207.tar.gz">Obtain cgic: gzipped tar file</a>
<li><a href="http://www.boutell.dev/cgic/cgic207.zip">Obtain cgic: .ZIP file</a>
</ul>
<h3><a name="build">Building cgic: a sample application</a></h3>
The sample application 'cgictest.c' is provided as part of the
cgic distribution. This CGI program displays an input form, 
accepts a submission, and then displays what was submitted.
In the process essentially all of cgic's features are tested.
<p>
On a Unix system, you can build cgictest simply by typing
'make cgictest.cgi'. cgic.c and cgictest.c will be compiled and linked
together to produce the cgictest application. Under non-Unix
operating systems, you will need to create and compile an appropriate
project containing the files cgic.c and cgictest.c. 
<p>
<strong>IMPORTANT:</strong> after compiling cgictest.cgi, you will
need to place it in a location on your server system which is
designated by your server administrator as an appropriate location
for CGI scripts. Some servers are configured to recognize any
file ending in .cgi as a CGI program when it is found in any
subdirectory of the server's web space, but this is not always
the case! The right locations for CGI
programs vary greatly from one server to another. Resolving
this issue is between you, your web server administrator,
and your web server documentation. Before submitting a bug
report for cgic, make certain that the CGI example programs
which came with your server <em>do</em> work for you. Otherwise
it is very likely that you have a server configuration problem.
<p>
Once you have moved cgictest.cgi (or cgictest.exe, under Windows)
to an appropriate cgi directory,
use the web browser of your choice to access the URL at which
you have installed it 
(for instance, <code>www.mysite.com/cgi-bin/cgictest.cgi</code>).
Fill out the various fields in any manner you wish, then
select the SUBMIT button.
<p>
If all goes well, cgictest.cgi will respond with a page which
indicates the various settings you submitted. If not,
please reread the section above regarding the correct location in
which to install your CGI program on your web server.
<h3><a name="nocompile">What to do if it won't compile</a></h3>
<ul>
<li><strong>Are you using Visual C++ or Borland C++? Did you forget to add
cgic.c to your project?</strong>
<li><strong>Make sure you are using an ANSI C or C++ compiler.</strong>
(All of the Windows compilers are ANSI C compliant.)
</ul>
If none of the above proves effective, please see the
section regarding <a href="#support">support</a>.
<h3><a name="howto">How to write a cgic application</a></h3>
<em>Note: </em> All cgic applications must be linked to the cgic.c module
itself. How to do this depends on your operating system; under Unix,
just use the provided Makefile as an example.
<p>
Since all CGI applications must perform certain initial
tasks, such as parsing form data and examining
environment variables, the cgic library provides its
own main() function. When you write applications that
use cgic, you will begin your own programs by writing
a cgiMain() function, which cgic will invoke when
the initial cgi work has been successfully completed. Your
program must also be sure to #include the file cgic.h.
<p>
<strong>Important:</strong> if you write your own main()
function, your program will not link properly. Your own
code should begin with cgiMain(). The library
provides main() for you. (Those who prefer different behavior
can easily modify cgic.c.)
<p>
Consider the cgiMain function of cgictest.c:
<p>
<PRE>
int cgiMain() {
#ifdef DEBUG
  LoadEnvironment();
#endif /* DEBUG */
  /* Load a previously saved CGI scenario if that button
    has been pressed. */
  if (cgiFormSubmitClicked("loadenvironment") == cgiFormSuccess) {
    LoadEnvironment();
  }
  /* Set any new cookie requested. Must be done *before*
    outputting the content type. */
  CookieSet();
  /* Send the content type, letting the browser know this is HTML */
  cgiHeaderContentType("text/html");
  /* Top of the page */
  fprintf(cgiOut, "&lt;HTML&gt;&lt;HEAD&gt;\n");
  fprintf(cgiOut, "&lt;TITLE&gt;cgic test&lt;/TITLE&gt;&lt;/HEAD&gt;\n");
  fprintf(cgiOut, "&lt;BODY&gt;&lt;H1&gt;cgic test&lt;/H1&gt;\n");
  /* If a submit button has already been clicked, act on the 
    submission of the form. */
  if ((cgiFormSubmitClicked("testcgic") == cgiFormSuccess) ||
    cgiFormSubmitClicked("saveenvironment") == cgiFormSuccess)
  {
    HandleSubmit();
    fprintf(cgiOut, "&lt;hr&gt;\n");
  }
  /* Now show the form */
  ShowForm();
  /* Finish up the page */
  fprintf(cgiOut, "&lt;/BODY&gt;&lt;/HTML&gt;\n");
  return 0;
}
</PRE>
Note the DEBUG #ifdef. If DEBUG is defined at compile time, either by
inserting the line "#define DEBUG 1" into the program or by setting
it in the Makefile or other development environment, then the
LoadEnvironment function is invoked. This function calls 
<a href="#cgiReadEnvironment">cgiReadEnvironment()</a> 
to restore a captured CGI environment for debugging purposes. See
also the discussion of the <a href="#debug">capture</a> program, which is
provided for use in CGI debugging. Because this is a test program,
the <a href="#cgiFormSubmitClicked">cgiFormSubmitClicked</a> function is
also called to check for the use of a button that requests the reloading
of a saved CGI environment. A completed CGI program typically would
never allow the end user to make that decision.
<h4>Setting Cookies</h4>
Next, one of the cgiHeader functions should be called.
This particular program demonstrates many features, including
the setting of cookies. If the programmer wishes to set a cookie,
the cookie-setting function must be called
first, before other headers are output. This is done by the
CookieSet() function of cgictest.c:
<pre>
void CookieSet()
{
  char cname[1024];
  char cvalue[1024];
  /* Must set cookies BEFORE calling 
    cgiHeaderContentType */
  cgiFormString("cname", cname, sizeof(cname));  
  cgiFormString("cvalue", cvalue, sizeof(cvalue));  
  if (strlen(cname)) {
    /* Cookie lives for one day (or until 
      browser chooses to get rid of it, which 
      may be immediately), and applies only to 
      this script on this site. */  
    cgiHeaderCookieSet(cname, cvalue,
      86400, cgiScriptName, cgiServerName,
      cgiCookieHttpOnly | cgiCookieSameSiteStrict);
  }
}
</pre>
Since this is a test program, the <a href="#cgiFormString">cgiFormString</a> 
function is used to fetch the name and value from the form previously filled
in by the user. Normally, cookie names and values are chosen to meet the
needs of the programmer and provide a means of identifying the same
user again later.
<p>
The <a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a>
function sets the cookie by requesting that the web browser store it.
<strong>There is never any guarantee that this will happen!</strong>
Many browsers reject cookies completely; others do not necessarily keep
them as long as requested or return them with their values intact.
Always code defensively when using cookies.
<p>
The cname and cvalue parameters are of course the name and value for
the cookie. The third argument is the time, in seconds, that the
cookie should "live" on the browser side before it expires; in this
case it has been set to 86,400 seconds, which is exactly one day. 
<strong>The browser may or may not respect this setting, as with everything
else about cookies.</strong>
<p>
The fourth argument identifies the "path" within the web site for which
the cookie is considered valid. A cookie that should be sent back
for every access to the site should be set with a path of <code>/</code>.
In this case the cookie is relevant only to the CGI program itself, so
<code><a href="#cgiScriptName">cgiScriptName</a></code> (the URL of the CGI program, not including the
domain name) is sent. Similarly, a cookie can be considered relevant
to a single web site or to an entire domain, such as 
<code>www.boutell.dev</code> or the entire <code>.boutell.dev</code>
domain. In this case, the current site on which the program is running
is the only relevant site, so <code><a href="#cgiServerName">cgiServerName</a></code> is used
as the domain.
<p>
The sixth argument sets extra security options, for example <i>HttpOnly</i> or
<i>SameSite=Strict</i> to prevent cross-site-scripting attacks.
<h4>Outputting the Content Type Header</h4>
Next, <a href="#cgiHeaderContentType">cgiHeaderContentType()</a> is 
called to indicate the MIME type of the document being output, in this case 
"text/html" (a normal HTML document). A few other common MIME types are
"image/gif", "image/jpeg" and "audio/wav". 
<p>
Note that <a href="#cgiHeaderStatus">cgiHeaderStatus()</a> or 
<a href="#cgiHeaderLocation">cgiHeaderLocation()</a> could have
been invoked instead to output an error code or redirect the
request to a different URL. Only one of the cgiHeader functions
should be called in a single execution of the program.
<p>
<strong>Important:</strong> one of the cgiHeader functions,
usually <a href="#cgiHeaderContentType">cgiHeaderContentType()</a>, 
<em>must</em> be invoked before outputting any other
response to the user. Otherwise, the result will not be a valid
document and the browser's behavior will be unpredictable.
You may, of course, output your own ContentType and other
header information to <a href="#cgiOut">cgiOut</a> if you prefer. The cgiHeader functions
are provided as a convenience.
<h4>Handling Form Submissions</h4>
Like many CGI programs, cgictest makes decisions about the way it
should behave based on whether various submit buttons have been clicked.
When either the testcgic or saveenvironment button is present, cgictest
invokes the HandleSubmit function, which invokes additional functions to
handle various parts of the form:
<pre>
void HandleSubmit()
{
  Name();
  Address();
  Hungry();
  Temperature();
  Frogs();
  Color();
  Flavors();
  NonExButtons();
  RadioButtons();
  File();
  Entries();
  Cookies();
  /* The saveenvironment button, in addition to submitting 
    the form, also saves the resulting CGI scenario to 
    disk for later replay with the 'load saved environment' 
    button. */
  if (cgiFormSubmitClicked("saveenvironment") == cgiFormSuccess) {
    SaveEnvironment();
  }
}
</pre>
<h4>Handling Text Input</h4>
The Name() function of cgictest is shown below, in its simplest
possible form:
<PRE>
void Name() {
        char name[81];
        <a href="#cgiFormStringNoNewlines">cgiFormStringNoNewlines</a>("name", name, 81);
        fprintf(cgiOut, "Name: ");
        cgicHtmlEscape(name);
        fprintf(cgiOut, "<BR>\n");
}
</PRE>
The purpose of this function is to retrieve and display the name that was
input by the user. Since the programmer has decided that names should
be permitted to have up to 80 characters, a buffer of 81 characters
has been declared (allowing for the final null character). 
The <a href="#cgiFormStringNoNewlines">cgiFormStringNoNewlines()</a>
function is then invoked to retrieve the name and ensure that
carriage returns are not present in the name (despite the
incorrect behavior of some web browsers). The first argument
is the name of the input field in the form, the second argument
is the buffer to which the data should be copied, and the third
argument is the size of the buffer. cgic will never write beyond
the size of the buffer, and will always provide a null-terminated
string in response; if the buffer is too small, the string will
be shortened. If this is not acceptable, the
<a href="#cgiFormStringSpaceNeeded">cgiFormStringSpaceNeeded()</a>
function can be used to check the amount of space needed; the
return value of cgiFormStringNoNewlines() can also be checked
to determine whether truncation occurred. See
the full description of <a href="#cgiFormStringNoNewlines">
cgiFormStringNoNewlines()</a>.
<h4>Handling Output</h4>
Note that Name() writes its HTML output to <a href="#cgiOut">cgiOut</a>, not
to stdout.
<p>
The actual name submitted by the user may or may not contain
characters that have special meaning in HTML, specifically the
the <code>&lt;</code>, <code>&gt;</code>, and <code>&amp;</code> characters.
The <a href="#cgiHtmlEscape">cgiHtmlEscape</a> function is used to output
the user-entered name with any occurrences of these characters
correctly escaped as <code>&amp;lt;</code>, <code>&amp;gt;</code>, 
and <code>&amp;amp;</code>.
<p>
<strong>Important:</strong> <a href="#cgiOut">cgiOut</a> is normally equivalent
to stdout, and there is no performance penalty for using it.
It is recommended that you write output to <a href="#cgiOut">cgiOut</a> to ensure compatibility
with modified versions of the cgic library for special
environments that do not provide stdin and stdout for
each cgi connection.
<p>
Note that, for text input areas in which carriage returns <em>are</em>
desired, the function <a href="#cgiFormString">cgiFormString</a>
should be used instead. cgiFormString ensures that line breaks
are always represented by a single carriage return (ascii decimal 13),
making life easier for the programmer. See the source code to
the Address() function of cgictest.c for an example.
<h4>Handling Single Checkboxes</h4>
Consider the Hungry() function, which determines whether
the user has selected the "hungry" checkbox:
<PRE>
void Hungry() {
        if (<a href="#cgiFormCheckboxSingle">cgiFormCheckboxSingle</a>("hungry") == <a href="#cgiFormSuccess">cgiFormSuccess</a>) {
                fprintf(cgiOut, "I'm Hungry!&lt;BR&gt;\n");
        } else {
                fprintf(cgiOut, "I'm Not Hungry!&lt;BR&gt;\n");
        }
}
</PRE>
This function takes advantage of the
<a href="#cgiFormCheckboxSingle">cgiFormCheckboxSingle()</a> function, which
determines whether a single checkbox has been selected. 
cgiFormCheckboxSingle() accepts the name attribute of the checkbox
as its sole argument and returns <a href="#cgiFormSuccess">
cgiFormSuccess</a> if the checkbox is selected, or 
<a href="#cgiFormNotFound">cgiFormNotFound</a> if it is not.
If multiple checkboxes with the same name are in use,
consider the <a href="#cgiFormCheckboxMultiple">
cgiFormCheckboxMultiple()</a> and 
<a href="#cgiFormStringMultiple">cgiFormStringMultiple()</a>
functions.
<h4>Handling Numeric Input</h4>
Now consider the Temperature() function, which retrieves
a temperature in degrees (a floating-point value) and ensures
that it lies within particular bounds:
<PRE>
void Temperature() {
        double temperature;
        <a href="#cgiFormDoubleBounded">cgiFormDoubleBounded</a>("temperature", &amp;temperature, 80.0, 120.0, 98.6);
        fprintf(<a href="#cgiOut">cgiOut</a>, "My temperature is %f.&lt;BR&gt;\n", temperature);
}
</PRE>
The temperature is retrieved by the function 
<a href="#cgiFormDoubleBounded">cgiFormDoubleBounded()</a>. The first
argument is the name of the temperature input field in the form;
the second argument points to the address of the variable that will 
contain the result. The next two arguments are the lower and upper
bounds, respectively. The final argument is the default value to
be returned if the user did not submit a value.
<p>
This function always retrieves a reasonable value within the
specified bounds; values above or below bounds are constrained
to fit the bounds. However, the return value of
cgiFormDoubleBounded can be checked to make sure the
actual user entry was in bounds, not blank, and so forth;
see the description of <a href="#cgiFormDoubleBounded">
cgiFormDoubleBounded()</a> for more details. If bounds checking
is not desired, consider using <a href="#cgiFormDouble">
cgiFormDouble()</a> instead.
<p>
Note that, for integer input, the functions
<a href="#cgiFormInteger">cgiFormInteger</a> and
<a href="#cgiFormIntegerBounded">cgiFormIntegerBounded</a>
are available. The behavior of these functions is similar to
that of their floating-point counterparts above.
<h4>Handling Single-Choice Input</h4>
The &lt;SELECT&gt; tag of HTML is used to provide the user with
several choices. Radio buttons and checkboxes can also be used
when the number of choices is relatively small. Consider
the Color() function of cgictest.c:
<PRE>
char *colors[] = {
        "Red",
        "Green",
        "Blue"
};

void Color() {
        int colorChoice;
        <a href="#cgiFormSelectSingle">cgiFormSelectSingle</a>("colors", colors, 3, &amp;colorChoice, 0);
        fprintf(<a href="#cgiOut">cgiOut</a>, "I am: %s&lt;BR&gt;\n", colors[colorChoice]);
}
</PRE>
This function determines which of several colors the user chose
from a &lt;SELECT&gt; list in the form. An array of colors is
declared; the <a href="#cgiFormSelectSingle">cgiFormSelectSingle()</a>
function is then invoked to determine which, if any, of those choices
was selected. The first argument indicates the name of the input
field in the form. The second argument points to the list of
acceptable colors. The third argument indicates the number of
entries in the color array. The fourth argument points to the
variable which will accept the chosen color, and the last argument
indicates the index of the default value to be set if no
selection was submitted by the browser. 
<p>
<a href="#cgiFormSelectSingle">cgiFormSelectSingle()</a> will
always indicate a reasonable selection value. However, if
the programmer wishes to know for certain that a value was
actually submitted, that the value submitted was a legal
response, and so on, the return value of cgiFormSelectSingle()
can be consulted. See the full description of
<a href="#cgiFormSelectSingle">cgiFormSelectSingle()</a> for
more information.
<p>
Note that radio button groups and &lt;SELECT&gt; lists can both
be handled by this function. If you are processing radio
button groups, you may prefer to invoke 
<a href="#cgiFormRadio">cgiFormRadio()</a>, which functions
identically. 
<p>
<em>"What if I won't know the acceptable choices at runtime?"</em>
<p>
If the acceptable choices aren't known <em>until</em> runtime,
one can simply load the choices from disk. But if the acceptable
choices aren't fixed at all (consider a list of country names;
new names may be added to the form at any time and it is
inconvenient to also update program code or a separate list
of countries), simply invoke 
<a href="#cgiFormStringNoNewlines">cgiFormStringNoNewlines()</a>
instead to retrieve the string directly. Keep in mind that, if
you do so, validating the response to make sure it is
safe and legitimate becomes a problem for your own
program to solve. The advantage of cgiFormSelectSingle() is that invalid 
responses are never returned.
<p>
To handle multiple-selection &lt;SELECT&gt; lists and
groups of checkboxes with the same name, see the
discussion of the NonExButtons() function of cgictest.c, immediately below.
<h4>Handling Multiple-Choice Input</h4>
Consider the first half of the NonExButtons() function of cgictest.c:
<PRE>
char *votes[] = {
  "A",
  "B",
  "C",
  "D"
};

void NonExButtons() {
  int voteChoices[4];
  int i;
  int result;  
  int invalid;

  char **responses;

  /* Method #1: check for valid votes. This is a good idea,
    since votes for nonexistent candidates should probably
    be discounted... */
  fprintf(<a href="#cgiOut">cgiOut</a>, "Votes (method 1):&lt;BR&gt;\n");
  result = <a href="#cgiFormCheckboxMultiple">cgiFormCheckboxMultiple</a>("vote", votes, 4, 
    voteChoices, &amp;invalid);
  if (result == <a href="#cgiFormNotFound">cgiFormNotFound</a>) {
    fprintf(<a href="#cgiOut">cgiOut</a>, "I hate them all!&lt;p&gt;\n");
  } else {  
    fprintf(<a href="#cgiOut">cgiOut</a>, "My preferred candidates are:\n");
    fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;ul&gt;\n");
    for (i=0; (i &lt; 4); i++) {
      if (voteChoices[i]) {
        fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;li&gt;%s\n", votes[i]);
      }
    }
    fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;/ul&gt;\n");
  }
</PRE>
This function takes advantage of
<a href="#cgiFormCheckboxMultiple">cgiFormCheckboxMultiple()</a>,
which is used to identify one or more selected checkboxes with 
the same name. This function performs identically to
<a href="#cgiFormSelectMultiple">cgiFormSelectMultiple()</a>.
That is, &lt;SELECT&gt; tags with the MULTIPLE attribute are handled
just like a group of several checkboxes with the same name.
<p>
The first argument to <a href="#cgiFormCheckboxMultiple">
cgiFormCheckboxMultiple()</a> is the name given to all
checkbox input fields in the group. The second argument
points to an array of legitimate values; these should
correspond to the VALUE attributes of the checkboxes
(or OPTION tags in a &lt;SELECT&gt; list). The third argument
indicates the number of entries in the array of
legitimate values. The fourth argument points to
an array of integers with the same number of entries
as the array of legitimate values; each entry
will be set true if that checkbox or option was selected,
false otherwise.
<p>
The last argument points to an integer which will be set to the 
number of invalid responses (responses not in the array of
valid responses) that were submitted. If this value is not
of interest, the last argument may be a null pointer (0).
<p>
Note that the return value of cgiFormCheckboxMultiple is
inspected to determine whether any choices at all were
set. See the full description of
<a href="#cgiFormCheckboxMultiple">cgiFormCheckboxMultiple</a>
for other possible return values. 
<p>
<em>"What if I won't know the acceptable choices at runtime?"</em>
<p>
If the acceptable choices aren't known <em>until</em> runtime,
one can simply load the choices from disk. But if the acceptable
choices aren't fixed at all (consider a list of ice cream flavors;
new names may be added to the form at any time and it is
inconvenient to also update program code or a separate list
of countries), a more dynamic approach is needed. Consider
the second half of the NonExButtons() function of cgictest.c:
<PRE>
  /* Method #2: get all the names voted for and trust them.
    This is good if the form will change more often
    than the code and invented responses are not a danger
    or can be checked in some other way. */
  fprintf(<a href="#cgiOut">cgiOut</a>, "Votes (method 2):&lt;BR&gt;\n");
  result = <a href="#cgiFormStringMultiple">cgiFormStringMultiple</a>("vote", &amp;responses);
  if (result == <a href="#cgiFormNotFound">cgiFormNotFound</a>) {  
    fprintf(<a href="#cgiOut">cgiOut</a>, "I hate them all!&lt;p&gt;\n");
  } else {
    int i = 0;
    fprintf(<a href="#cgiOut">cgiOut</a>, "My preferred candidates are:\n");
    fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;ul&gt;\n");
    while (responses[i]) {
      fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;li&gt;%s\n", responses[i]);
      i++;
    }
    fprintf(<a href="#cgiOut">cgiOut</a>, "&lt;/ul&gt;\n");
  }
  /* We must be sure to free the string array or a memory
    leak will occur. Simply calling free() would free
    the array but not the individual strings. The
    function cgiStringArrayFree() does the job completely. */  
  <A HREF="#cgiStringArrayFree">cgiStringArrayFree</a>(responses);
}
</PRE>
This code excerpt demonstrates an alternate means of retrieving
a list of choices. The function
<a href="#cgiFormStringMultiple">cgiFormStringMultiple()</a> is used
to retrieve an array consisting of all the strings submitted
for with a particular input field name. This works both for
&lt;SELECT&gt; tags with the MULTIPLE attribute and for 
groups of checkboxes with the same name. 
<P>
The first argument to <a href="#cgiFormStringMultiple">
cgiFormStringMultiple()</a> is the name of the input field or
group of input fields in question. The second argument should
be the address of a pointer to a pointer to a string, which
isn't as bad as it sounds. Consider the following simple call
of the function:
<PRE>
/* An array of strings; each C string is an array of characters */
char **responses; 

<a href="#cgiFormStringMultiple">cgiFormStringMultiple</a>("vote", &amp;responses);
</PRE>
<em>"How do I know how many responses there are?"</em>
<p>
After the call, the last entry in the string array will be
a null pointer. Thus the simple loop:
<PRE>
int i = 0;
while (responses[i]) {
  /* Do something with the string responses[i] */
  i++;
}
</PRE>
can be used to walk through the array until the last
entry is encountered.
<p>
<strong>Important:</strong> the 
<a href="#cgiFormStringMultiple">cgiFormStringMultiple</a> function
returns a pointer to <strong>allocated memory</strong>. Your code
should not modify the strings in the responses array or the responses
array itself; if modification is needed, the strings should be
copied. When your code is done examining the responses array,
you <strong>MUST</strong> call <a href="#cgiStringArrayFree">
cgiStringArrayFree()</a> with the array as an argument to free the memory 
associated with the array. Otherwise, the memory will not be available 
again until the program exists. <strong>Don't</strong> just call the 
free() function; if you do, the individual strings will not be freed.
<h4>Accessing Uploaded Files</h4>
CGIC provides functions to access files that have been uploaded
as part of a form submission. <strong>IMPORTANT: you MUST</strong> set
the <code>enctype</code> attribute of your <code>form</code> tag
to <code>multipart/form-data</code> for this feature to work! For an
example, see the <a href="#ShowForm">ShowForm</a> function of 
cgictest.c, examined below.
<p>
The <code>File</code> function of cgictest.c takes care of 
receiving uploaded files:
<pre>
void File()
{
  cgiFilePtr file;
  char name[1024];
  char contentType[1024];
  char buffer[1024];
  int size;
  int got;
  if (cgiFormFileName("file", name, sizeof(name)) != 
    cgiFormSuccess) 
  {
    printf("&lt;p&gt;No file was uploaded.&lt;p&gt;\n");
    return;
  } 
        fprintf(cgiOut, "The filename submitted was: ");
        cgiHtmlEscape(name);
        fprintf(cgiOut, "&lt;p&gt;\n");
        cgiFormFileSize("file", &size);
        fprintf(cgiOut, "The file size was: %d bytes&lt;p&gt;\n", size);
        cgiFormFileContentType("file", contentType, sizeof(contentType));
        fprintf(cgiOut, "The alleged content type of the file was: ");
        cgiHtmlEscape(contentType);
        fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Of course, this is only the claim the browser "
    "made when uploading the file. Much like the filename, "
    "it cannot be trusted.&lt;p&gt;\n");
  fprintf(cgiOut, "The file's contents are shown here:&lt;p&gt;\n");
  if (cgiFormFileOpen("file", &file) != cgiFormSuccess) {
    fprintf(cgiOut, "Could not open the file.&lt;p&gt;\n");
    return;
  }
  fprintf(cgiOut, "&lt;pre&gt;\n");
  while (cgiFormFileRead(file, buffer, sizeof(buffer), &got) ==
    cgiFormSuccess)
  {
    cgiHtmlEscapeData(buffer, got);
  }
  fprintf(cgiOut, "&lt;/pre&gt;\n");
  cgiFormFileClose(file);
}
</pre>
First, the File function checks to determine the filename that was
submitted by the user. <strong>VERY IMPORTANT: this filename may or
may not bear any relation to the real name of the file on the user's
computer, may be deliberately manipulated with malicious intent,</strong>
and should not be used for <strong>any</strong> purpose unless you have
determined that its content is safe for your intended use and will not,
at the very least, overwrite another file of importance to you, especially if
you intend to use it as a file name on the server side. The cgic library
itself does not use this file name for temporary storage.
<p>
If the <a href="#cgiFormFileName">cgiFormFileName</a> function does
not succeed, no file was uploaded.
<p>
Next, the <a href="#cgiFormFileSize">cgiFormFileSize</a> function is called
to determine the size of the uploaded file, in bytes.
<p>
The File function then proceeds to query the content type of the uploaded
file.  Files uploaded by the user have their own content type information, 
which may be useful in determining whether the file is an image, HTML document,
word processing document, or other type of file. However,
<strong>as with the filename and any other claim made by the browser,
this information should not be blindly trusted.</strong> The browser
may upload a file with the name <code>picture.jpg</code> and the
content type <code>image/jpeg</code>, but this does not guarantee that the
actual file will contain a valid JPEG image suitable for display.
<p>
The content type submitted by the browser can be queried using the
<a href="#cgiFormFileContentType">cgiFormFileContentType</a> function.
<p>
Of course, CGIC also provides access to the actual uploaded file. 
First, the programmer calls <a href="#cgiFormFileOpen">cgiFormFileOpen</a>,
passing the address of a <code>cgiFilePtr</code> object. If this function
succeeds, the <code>cgiFilePtr</code> object becomes valid, and can be
used in subsequent calls to <a href="#cgiFormFileRead">cgiFormFileRead</a>.
Notice that the number of bytes read may be less than the number requested,
in particular on the last successful call before cgiFormFileRead begins
to return <code>cgiFormEOF</code>. When cgiFormFileRead no longer returns 
cgiFormSuccess, 
the programmer calls <a href="#cgiFormClose">cgiFormFileClose</a> to
release the <code>cgiFilePtr</code> object.
<p>
The uploaded file data may contain anything, including binary data,
null characters, and so on. The example program uses the 
<a href="#cgiHtmlEscapeData">cgiHtmlEscapeData</a> function to output the
data with any special characters that have meaning in HTML escaped.
Most programs will save the uploaded information to a server-side file or
database.
<h4>Fetching All Form Entries</h4>
From time to time, the programmer may not know the names of all
form fields in advance. In such situations it is convenient to
use the <a href="#cgiFormEntries">cgiFormEntries</a> function.
The Entries function of cgictest.c demonstrates the use of
cgiFormEntries:
<pre>
void Entries()
{
        char **array, **arrayStep;
        fprintf(cgiOut, "List of All Submitted Form Field Names:&lt;p&gt;\n");
        if (cgiFormEntries(&array) != cgiFormSuccess) {
                return;
        }
        arrayStep = array;
        fprintf(cgiOut, "&lt;ul&gt;\n");
        while (*arrayStep) {
                fprintf(cgiOut, "&lt;li&gt;");
                cgiHtmlEscape(*arrayStep);
                fprintf(cgiOut, "\n");
                arrayStep++;
        }
        fprintf(cgiOut, "&lt;/ul&gt;\n");
        cgiStringArrayFree(array);
}
</pre>
The cgiFormEntries function retrieves an array of form field names.
This array consists of pointers to strings, with a final null pointer
to mark the end of the list. The above code illustrates one way of
looping through the returned strings. Note the final call to
<a href="#cgiStringArrayFree">cgiStringArrayFree</a>, which is
essential in order to return the memory used to store the strings
and the string array.
<h4>Retrieving Cookies</h4>
The Cookies function of cgictest.c displays a list of all cookies
submitted by the browser with the current form submission, along
with their values:
<pre>
void Cookies()
{
  char **array, **arrayStep;
  char cname[1024], cvalue[1024];
  fprintf(cgiOut, "Cookies Submitted On This Call, With Values "
    "(Many Browsers NEVER Submit Cookies):&lt;p&gt;\n");
  if (cgiCookies(&array) != cgiFormSuccess) {
    return;
  }
  arrayStep = array;
  fprintf(cgiOut, "&lt;table border=1&gt;\n");
  fprintf(cgiOut, "&lt;tr&gt;&lt;th&gt;Cookie&lt;th&gt;Value&lt;/tr&gt;\n");
  while (*arrayStep) {
    char value[1024];
    fprintf(cgiOut, "&lt;tr&gt;");
    fprintf(cgiOut, "&lt;td&gt;");
    cgiHtmlEscape(*arrayStep);
    fprintf(cgiOut, "&lt;td&gt;");
    cgiCookieString(*arrayStep, value, sizeof(value));
    cgiHtmlEscape(value);
    fprintf(cgiOut, "\n");
    arrayStep++;
  }
  fprintf(cgiOut, "&lt;/table&gt;\n");
  cgiFormString("cname", cname, sizeof(cname));  
  cgiFormString("cvalue", cvalue, sizeof(cvalue));  
  if (strlen(cname)) {
    fprintf(cgiOut, "New Cookie Set On This Call:&lt;p&gt;\n");
    fprintf(cgiOut, "Name: ");  
    cgiHtmlEscape(cname);
    fprintf(cgiOut, "Value: ");  
    cgiHtmlEscape(cvalue);
    fprintf(cgiOut, "&lt;p&gt;\n");
    fprintf(cgiOut, "If your browser accepts cookies "
      "(many do not), this new cookie should appear "
      "in the above list the next time the form is "
      "submitted.&lt;p&gt;\n"); 
  }
  cgiStringArrayFree(array);
}
</pre>
<strong>VERY IMPORTANT: YOUR BROWSER MIGHT NOT SUBMIT COOKIES,
EVER, REGARDLESS OF WHAT VALUES YOU ENTER INTO THE TEST FORM.</strong>
Many, many browsers are configured not to accept or send cookies;
others are configured to send them as little as possible to meet the
bare minimum requirements for entry into popular sites. Users will often
refuse your cookies; make sure your code still works in that situation!
<p>
The above code uses the <a href="#cgiCookies">cgiCookies</a> function
to retrieve a list of all currently set cookies as a null-terminated
array of strings. The <a href="#cgiCookieString">cgiCookieString</a>
function is then used to fetch the value associated with each cookie;
this function works much like <a href="#cgiFormString">cgiFormString</a>,
discussed earlier. Note that a cookie set as a part of the current
form submission process does not appear on this list immediately, as
it has not yet been sent back by the browser. It should appear on
future submissions, provided that the browser chooses to accept
and resend the cookie at all.
<h4>Displaying a Form That Submits to the Current Program</h4>
CGI programmers often need to display HTML pages as part of the output
of CGI programs; these HTML pages often contain forms which should submit
fields back to the same program they came from. Provided that your
web server is well-configured, this can be done conveniently using
the cgiScriptName environment variable, as shown below. Here is the
source code of the ShowForm function of cgictest.c:
<pre>
void ShowForm()
{
  fprintf(cgiOut, "&lt;!-- 2.0: multipart/form-data is required 
    "for file uploads. --&gt;");
  fprintf(cgiOut, "&lt;form method=\"POST\" "
    "enctype=\"multipart/form-data\" ");
  fprintf(cgiOut, "  action=\"");
  cgiValueEscape(cgiScriptName);
  fprintf(cgiOut, "\"&gt;\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Text Field containing Plaintext\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"text\" name=\"name\"&gt;Your Name\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Multiple-Line Text Field\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;textarea NAME=\"address\" ROWS=4 COLS=40&gt;\n");
  fprintf(cgiOut, "Default contents go here. \n");
  fprintf(cgiOut, "&lt;/textarea&gt;\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Checkbox\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"checkbox\" name=\"hungry\" checked&gt;Hungry\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Text Field containing a Numeric Value\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"text\" name=\"temperature\" value=\"98.6\"&gt;\n");
  fprintf(cgiOut, "Blood Temperature (80.0-120.0)\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Text Field containing an Integer Value\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"text\" name=\"frogs\" value=\"1\"&gt;\n");
  fprintf(cgiOut, "Frogs Eaten\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "Single-SELECT\n");
  fprintf(cgiOut, "&lt;br&gt;\n");
  fprintf(cgiOut, "&lt;select name=\"colors\"&gt;\n");
  fprintf(cgiOut, "&lt;option value=\"Red\"&gt;Red\n");
  fprintf(cgiOut, "&lt;option value=\"Green\"&gt;Green\n");
  fprintf(cgiOut, "&lt;option value=\"Blue\"&gt;Blue\n");
  fprintf(cgiOut, "&lt;/select&gt;\n");
  fprintf(cgiOut, "&lt;br&gt;\n");
  fprintf(cgiOut, "Multiple-SELECT\n");
  fprintf(cgiOut, "&lt;br&gt;\n");
  fprintf(cgiOut, "&lt;select name=\"flavors\" multiple&gt;\n");
  fprintf(cgiOut, "&lt;option value=\"pistachio\"&gt;Pistachio\n");
  fprintf(cgiOut, "&lt;option value=\"walnut\"&gt;Walnut\n");
  fprintf(cgiOut, "&lt;option value=\"creme\"&gt;Creme\n");
  fprintf(cgiOut, "&lt;/select&gt;\n");
  fprintf(cgiOut, "&lt;p&gt;Exclusive Radio Button Group: Age of "
    "Truck in Years\n");
  fprintf(cgiOut, "&lt;input type=\"radio\" name=\"age\" "
    "value=\"1\"&gt;1\n");
  fprintf(cgiOut, "&lt;input type=\"radio\" name=\"age\" "
    "value=\"2\"&gt;2\n");
  fprintf(cgiOut, "&lt;input type=\"radio\" name=\"age\" "
    "value=\"3\" checked&gt;3\n");
  fprintf(cgiOut, "&lt;input type=\"radio\" name=\"age\" "
    "value=\"4\"&gt;4\n");
  fprintf(cgiOut, "&lt;p&gt;Nonexclusive Checkbox Group: "
    "Voting for Zero through Four Candidates\n");
  fprintf(cgiOut, "&lt;input type=\"checkbox\" name=\"vote\" "
    "value=\"A\"&gt;A\n");
  fprintf(cgiOut, "&lt;input type=\"checkbox\" name=\"vote\" "
    "value=\"B\"&gt;B\n");
  fprintf(cgiOut, "&lt;input type=\"checkbox\" name=\"vote\" "
    "value=\"C\"&gt;C\n");
  fprintf(cgiOut, "&lt;input type=\"checkbox\" name=\"vote\" "
    "value=\"D\"&gt;D\n");
  fprintf(cgiOut, "&lt;p&gt;File Upload:\n");
  fprintf(cgiOut, "&lt;input type=\"file\" name=\"file\" "
    "value=\"\"&gt; (Select A Local File)\n");
  fprintf(cgiOut, "&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;p&gt;Set a Cookie&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input name=\"cname\" "
    "value=\"\"&gt; Cookie Name\n");
  fprintf(cgiOut, "&lt;input name=\"cvalue\" "
    "value=\"\"&gt; Cookie Value&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"submit\" "
    "name=\"testcgic\" value=\"Submit Request\"&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"reset\" "
    "value=\"Reset Request\"&gt;\n");
  fprintf(cgiOut, "&lt;p&gt;Save the CGI Environment&lt;p&gt;\n");
  fprintf(cgiOut, "Pressing this button will submit the form, then "
    "save the CGI environment so that it can be replayed later "
    "by calling cgiReadEnvironment (in a debugger, for "
    "instance).&lt;p&gt;\n");
  fprintf(cgiOut, "&lt;input type=\"submit\" name=\"saveenvironment\" "
    "value=\"Save Environment\"&gt;\n");
  fprintf(cgiOut, "&lt;/form&gt;\n");
}
</pre>
Note the use of <code>enctype="multipart/form-data"</code> in the
<code>FORM</code> tag. This is absolutely required if the form
will contain file upload fields, as in the above example. Most
browsers will not even attempt file uploads without the
presence of this attribute.
<h4>Examining CGI environment variables</h4>
The CGI standard specifies a number of environment variables
which are set by the server. However, servers are somewhat
unpredictable as to whether these variables will be null or
point to empty strings when an environment variable is not set.
Also, in order to allow the programmer to restore saved
CGI environments, the cgic library needs have a way of insulating
the programmer from the actual environment variables.
<p>
Instead of calling getenv() to determine the value of a
variable such as HTTP_USER_AGENT (the browser software being used),
always use the
<a href="#variables">cgic copies of the environment variables</a>,
which are always valid C strings (they are never null, although
they may point to an empty string). For instance, the cgic
variable containing the name of the browser software is
<a href="#cgiUserAgent">cgiUserAgent</a>. The referring URL appears
in the variable <a href="#cgiReferrer">cgiReferrer</a>.
<h3><a name="images">How can I generate images from my cgic application?</a></h3>
cgic can be used in conjunction with the
<a href="https://libgd.github.io/">gd graphics library</a>, which
can produce GIF images on the fly.
<p>
The following short sample program hints at the possibilities:
<pre>
#include "cgic.h"
#include "gd.h"

char *colors[] = {
  "red", "green", "blue"
};

#define colorsTotal 3

int cgiMain() {
  int colorChosen;
  gdImagePtr im;
  int r, g, b;
  /* Use gd to create an image */
  im = gdImageCreate(64, 64);
  r = gdImageColorAllocate(im, 255, 0, 0);  
  g = gdImageColorAllocate(im, 0, 255, 0);  
  b = gdImageColorAllocate(im, 0, 0, 255);  
  /* Now use cgic to find out what color the user requested */
  <a href="#cgiFormSelectSingle">cgiFormSelectSingle</a>("color", 3, &amp;colorChosen, 0);  
  /* Now fill with the desired color */
  switch(colorChosen) {
    case 0:
    gdImageFill(im, 32, 32, r);
    break;
    case 1:
    gdImageFill(im, 32, 32, g);
    break;
    case 2:
    gdImageFill(im, 32, 32, b);
    break;
  }  
  /* Now output the image. Note the content type! */
  cgiHeaderContentType("image/gif");
  /* Send the image to cgiOut */
  gdImageGif(im, cgiOut);
  /* Free the gd image */
  gdImageDestroy(im);
  return 0;
}
</pre>
Note that this program would need to be linked with both cgic.o
and libgd.a. Often programs of this type respond to one
cgiPathInfo value or set of form fields by returning an HTML page 
with an inline image reference that, in turn, generates a GIF image.
<h3><a name="debug">Debugging CGI applications: using capture</a></h3>
Debugging CGI applications can be a painful task. Since CGI applications
run in a special environment created by the web server, it is difficult
to execute them in a debugger. However, the cgic library provides a way 
of capturing "live" CGI environments to a file, and also provides a way
to reload saved environments. 
<p>
The provided program 'capture.c' can be used to capture CGI
environments. Just change the first line of the cgiMain() function
of capture.c to save the CGI environment to a filename appropriate
on your system and type 'make capture'. Then place capture in your
cgi directory and set the form action or other link you want to test
to point to it. When the form submission or other link takes place,
capture will write the CGI environment active at that time to
the filename you specified in the source. The
<a href="#cgiReadEnvironment">cgiReadEnvironment()</a> function can then 
be invoked on the same filename at the beginning of the cgiMain() function 
of the application you want to test in order to restore the captured 
environment.  You can then execute your program in the debugger of your choice,
and it should perform exactly as it would have performed had
it been launched by the actual web server, including file uploads,
cookies and all other phenomena within the purview of cgic.
<p>
<strong>Important:</strong> Make sure you specify the full path, as the
current working directory of a CGI script may not be what you
think it is!
<p>
<strong>Even More Important:</strong> If you call getenv() yourself
in your code, instead of using the provided <a href="#variables">
cgic copies of the CGI environment variables</a>, you will
<em>not</em> get the values you expect when running with
a saved CGI environment. Always use the cgic variables instead
of calling getenv().
<h3><a name="functions">cgic function reference</a></h3>
<dl>
<br><dt><strong><a name="cgiFormString">cgiFormResultType cgiFormString(
  char *name, char *result, int max)</a>
</strong><br><dd>cgiFormString attempts to retrieve the string sent for the
  specified input field. The text will be copied into
  the buffer specified by result, up to but not
  exceeding max-1 bytes; a terminating null is then
  added to complete the string. Regardless of the newline
  format submitted by the browser, cgiFormString always
  encodes each newline as a single line feed (ascii decimal 10); as
  a result the final string may be slightly shorter than indicated
  by a call to <a href="#cgiFormStringSpaceNeeded">
  cgiFormStringSpaceNeeded</a> but will never be longer.
  cgiFormString returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if the string was 
  successfully retrieved, 
  <a href="#cgiFormTruncated">cgiFormTruncated</a> if the string was
  retrieved but was truncated to fit the buffer,
  cgiFormEmpty if the string was 
  retrieved but was empty, and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no 
  such input field was submitted. In the last case, 
  an empty string is copied to result. 
<br><br><dt><strong><a name="cgiFormStringNoNewlines">
cgiFormResultType cgiFormStringNoNewlines(
  char *name, char *result, int max)</a>
</strong><br><dd>
cgiFormStringNoNewlines() is exactly equivalent to <a href="#cgiFormString">
  cgiFormString()</a>, except
  that any carriage returns or line feeds that occur in the input
  will be stripped out. The use of this function is recommended
  for single-line text input fields, as some browsers will submit
  carriage returns and line feeds when they should not. 
<br><br><dt><strong><a name="cgiFormStringSpaceNeeded">
cgiFormResultType cgiFormStringSpaceNeeded(
  char *name, int *length)</a>
</strong><br><dd>
cgiFormStringSpaceNeeded() is used to determine the length of the input text 
  buffer needed to receive the contents of the specified input field. 
  This is useful if the programmer wishes to allocate sufficient memory 
  for input of arbitrary length. The actual length of the string 
  retrieved by a subsequent call to cgiFormString() may be slightly shorter
  but will never be longer than *result. On success, cgiFormStringSpaceNeeded() 
  sets the value pointed to by length to the number of bytes of data, 
  including the terminating null, and returns <a href="#cgiFormSuccess">cgiFormSuccess</a>. If no 
  value was submitted for the specified field, cgiFormStringSpaceNeeded sets 
  the value pointed to by length to 1 and returns <a href="#cgiFormNotFound">cgiFormNotFound</a>. 1 is
  set to ensure space for an empty string (a single null
  character) if cgiFormString is called despite the return value.

<br><br><dt><strong><a name="cgiFormStringMultiple">cgiFormResultType cgiFormStringMultiple(
  char *name, char ***ptrToStringArray)</a>
</strong><br><dd>cgiFormStringMultiple is useful in the unusual case in which several
  input elements in the form have the same name and, for whatever
  reason, the programmer does not wish to use the checkbox, radio 
  button and selection menu functions provided below. This is
  occasionally needed if the programmer cannot know 
  in advance what values might appear in a multiple-selection list
  or group of checkboxes on a form. The value pointed to
  by result will be set to a pointer to an array of strings; the last
  entry in the array will be a null pointer.  This array is allocated 
  by the CGI library. Important: when done working with the array,
  you must call cgiStringArrayFree() with the array pointer as the 
  argument.  cgiFormStringMultiple() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if at least
  one occurrence of the name is found, <a href="#cgiFormNotFound">cgiFormNotFound</a>
  if no occurrences are found, or cgiFormMemory if not enough
  memory is available to allocate the array to be returned.
  In all cases except the last, ptrToStringArray is set to point to a 
  valid array of strings, with the last element in the array being a 
  null pointer; in the out-of-memory case ptrToStringArray is set to 
  a null pointer.

<br><br><dt><strong><a name="cgiFormEntries">cgiFormResultType cgiFormEntries(
  char ***ptrToStringArray)</a>
</strong><br><dd>cgiFormEntries is useful when the programmer cannot know the names
  of all relevant form fields in advance. The value pointed to
  by result will be set to a pointer to an array of strings; the last
  entry in the array will be a null pointer.  This array is allocated 
  by the CGI library. Important: when done working with the array,
  you must call cgiStringArrayFree() with the array pointer as the 
  argument. cgiFormEntries() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> except in the event of an out of memory error.
  On success, ptrToStringArray is set to point to a 
  valid array of strings, with the last element in the array being a 
  null pointer; in the out-of-memory case ptrToStringArray is set to 
  a null pointer, and 
  <a href="#cgiFormOutOfMemory">cgiFormOutOfMemory</a> is returned.

<br><br><dt><strong><a name="cgiStringArrayFree">void cgiStringArrayFree(char **stringArray)
</a>
</strong><br><dd>
cgiStringArrayFree() is used to free the memory associated with
  a string array created by 
  <a href="#cgiFormStringMultiple">cgiFormStringMultiple()</a>,
  <a href="#cgiFormEntries">cgiFormEntries()</a>, or
  <a href="#cgiFormCookies">cgiFormCookies()</a>.
<br><br><dt><strong><a name="cgiFormInteger">cgiFormResultType cgiFormInteger(
  char *name, int *result, int defaultV)</a>
</strong><br><dd>cgiFormInteger() attempts to retrieve the integer sent for the
  specified input field. The value pointed to by result
  will be set to the value submitted. cgiFormInteger() returns 
  cgiFormSuccess if the value was successfully retrieved,
  cgiFormEmpty if the value submitted is an empty string,
  cgiFormBadType if the value submitted is not an integer,
  and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no such input field was submitted. 
  In the last three cases, the value pointed to by result
  is set to the specified default.
<br><br><dt><strong><a name="cgiFormIntegerBounded">
cgiFormResultType cgiFormIntegerBounded(
  char *name, int *result, int min, int max, int defaultV)</a>
</strong><br><dd>cgiFormIntegerBounded() attempts to retrieve the integer sent for the
  specified input field, and constrains the result to be within
  the specified bounds. The value pointed to by result
  will be set to the value submitted. cgiFormIntegerBounded() returns 
  cgiFormSuccess if the value was successfully retrieved,
  <a href="#cgiFormConstrained">cgiFormConstrained</a> if the value was out of bounds and result
  was adjusted accordingly, <a href="#cgiFormEmpty">cgiFormEmpty</a> if the value submitted is 
  an empty string, <a href="#cgiFormBadType">cgiFormBadType</a> if the value submitted is not an 
  integer, and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no such input field was submitted. 
  In the last three cases, the value pointed to by result
  is set to the specified default.

<br><br><dt><strong><a name="cgiFormDouble">cgiFormResultType cgiFormDouble(
  char *name, double *result, double defaultV)</a>
</strong><br><dd>cgiFormDouble attempts to retrieve the floating-point value sent for 
  the specified input field. The value pointed to by result
  will be set to the value submitted. cgiFormDouble returns 
  cgiFormSuccess if the value was successfully retrieved,
  cgiFormEmpty if the value submitted is an empty string,
  cgiFormBadType if the value submitted is not a number,
  and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no such input field was submitted. 
  In the last three cases, the value pointed to by result
  is set to the specified default. 
<br><br><dt><strong><a name="cgiFormDoubleBounded">
cgiFormResultType cgiFormDoubleBounded(
  char *name, double *result, double min, double max, 
  double defaultV)</a>
</strong><br><dd>
cgiFormDoubleBounded() attempts to retrieve the floating-point
  value  sent for the specified input field, and constrains the 
  result to be within the specified bounds. The value pointed to by 
  result will be set to the value submitted. cgiFormDoubleBounded() returns 
  cgiFormSuccess if the value was successfully retrieved,
  <a href="#cgiFormConstrained">cgiFormConstrained</a> if the value was out of bounds and result
  was adjusted accordingly, <a href="#cgiFormEmpty">cgiFormEmpty</a> if the value submitted is 
  an empty string, <a href="#cgiFormBadType">cgiFormBadType</a> if the value submitted is not a 
  number, and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no such input field was submitted. 
  In the last three cases, the value pointed to by result
  is set to the specified default. 

<br><br><dt><strong><a name="cgiFormSelectSingle">
cgiFormResultType cgiFormSelectSingle(
  char *name, char **choicesText, int choicesTotal, 
  int *result, int defaultV)</a>
</strong><br><dd>
cgiFormSelectSingle() retrieves the selection number associated with a
  &lt;SELECT&gt; element that does not allow multiple selections. name
  should identify the NAME attribute of the &lt;SELECT&gt; element. choicesText 
  should point to an array of strings identifying each choice; 
  choicesTotal should indicate the total number of choices. The value 
  pointed to by result will be set to the position of the actual choice
  selected within the choicesText array, if any, or to the value of 
  default, if no selection was submitted or an invalid selection was 
  made.  cgiFormSelectSingle() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if the value was
  successfully retrieved, <a href="#cgiFormNotFound">cgiFormNotFound</a> if no selection
  was submitted, and <a href="#cgiFormNoSuchChoice">cgiFormNoSuchChoice</a> if the selection
  does not match any of the possibilities in the choicesText array. 
<br><dt><strong>
<a name="cgiFormSelectMultiple">
cgiFormResultType cgiFormSelectMultiple(
  char *name, char **choicesText, int choicesTotal, 
  int *result, int *invalid)</a>
</strong><br><dd>cgiFormSelectMultiple() retrieves the selection numbers associated with a
  &lt;SELECT&gt; element that does allow multiple selections. name should
  identify the NAME attribute of the &lt;SELECT&gt; element. choicesText 
  should point to an array of strings identifying each choice; 
  choicesTotal should indicate the total number of choices. result
  should point to an array of integers with as many elements as there
  are strings in the choicesText array. For each choice in the
  choicesText array that is selected, the corresponding integer in
  the result array will be set to one; other entries in the result
  array will be set to zero. cgiFormSelectMultiple() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> 
  if at least one valid selection was successfully retrieved or
  cgiFormNotFound if no valid selections were submitted.
  The integer pointed to by invalid is set to the number of
  invalid selections that were submitted, which should be zero
  unless the form and the choicesText array do not agree.

<br><br><dt><strong>
<a name="cgiFormSubmitClicked">
cgiFormResultType cgiFormSubmitClicked(
  char *name)</a>
</strong><br><dd>
It is often desirable to know whether a particular submit button was clicked,
  when multiple submit buttons with different name attributes exist.
  cgiFormSubmitClicked is an alternative name for the 
  <a href="#cgiFormCheckboxSingle">cgiFormCheckboxSingle</a> function,
  which is suitable for testing whether a particular submit button
  was used.
<br><br><dt><strong>
<a name="cgiFormCheckboxSingle">
cgiFormResultType cgiFormCheckboxSingle(
  char *name)</a>
</strong><br><dd>
cgiFormCheckboxSingle determines whether the checkbox with the specified name
  is checked. cgiFormCheckboxSingle returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if the
  button is checked, <a href="#cgiFormNotFound">cgiFormNotFound</a> if the checkbox is
  not checked. cgiFormCheckboxSingle is intended for single
  checkboxes with a unique name; see below for functions to
  deal with multiple checkboxes with the same name, and
  with radio buttons.

<br><br><dt><strong><a name="cgiFormCheckboxMultiple">
cgiFormResultType cgiFormCheckboxMultiple(
  char *name, char **valuesText, int valuesTotal, 
  int *result, int *invalid)</a>
</strong><br><dd>cgiFormCheckboxMultiple() determines which checkboxes among a group
  of checkboxes with the same name are checked. This is distinct
  from radio buttons (see <a href="#cgiFormRadio">cgiFormRadio</a>). 
  valuesText 
  should point to an array of strings identifying the VALUE
  attribute of each checkbox; valuesTotal should indicate the total 
  number of checkboxes. result should point to an array of integers with 
  as many elements as there are strings in the valuesText array. For 
  each choice in the valuesText array that is selected, the corresponding
  integer in the result array will be set to one; other entries in the 
  result array will be set to zero. cgiFormCheckboxMultiple returns 
  cgiFormSuccess if at least one valid checkbox was checked or
  cgiFormNotFound if no valid checkboxes were checked.
  The integer pointed to by invalid is set to the number of
  invalid selections that were submitted, which should be zero
  unless the form and the valuesText array do not agree.
<br><br><dt><strong><a name="cgiFormRadio">
cgiFormResultType cgiFormRadio(
  char *name, char **valuesText, int valuesTotal, 
  int *result, int defaultV)</a>
</strong><br><dd>cgiFormRadio() determines which, if any, of a group of radio boxes with
  the same name was selected. valuesText should point to an array of 
  strings identifying the VALUE attribute of each radio box; 
  valuesTotal should indicate the total number of radio boxes. The value 
  pointed to by result will be set to the position of the actual choice 
  selected within the valuesText array, if any, or to the value of 
  default, if no radio box was checked or an invalid selection was 
  made. cgiFormRadio() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if a checked radio box was 
  found in the group, <a href="#cgiFormNotFound">cgiFormNotFound</a> if no box was checked, and 
  <a href="#cgiFormNoSuchChoice">cgiFormNoSuchChoice</a> if the radio box submitted does not match any of 
  the possibilities in the valuesText array.

<br><dt><strong><a name="cgiFormFileName">cgiFormResultType cgiFormFileName(
  char *name, char *fileName, int max)</a>
</strong><br><dd>cgiFormFileName attempts to retrieve the file name uploaded by the
  user for the specified form input field of type <code>file</code>. 
  <strong>NEVER, EVER TRUST THIS FILENAME TO BE REASONABLE AND
  SAFE FOR DIRECT USE ON THE SERVER SIDE.</strong>
  The text will be copied into
  the buffer specified by fileName, up to but not
  exceeding max-1 bytes; a terminating null is then
  added to complete the string. cgiFormFileName returns 
  <a href="#cgiFormSuccess">cgiFormSuccess</a> if the string was 
  successfully retrieved and was not empty,
  <a href="#cgiFormNoFileName">cgiFormNoFileName</a> if the string was 
  successfully retrieved but empty indicating that no file was uploaded,
  <a href="#cgiFormTruncated">cgiFormTruncated</a> if the string was
  retrieved but was truncated to fit the buffer,
  and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no 
  such input field was submitted. In the last case, 
  an empty string is copied to result. 
<br><dt><strong><a name="cgiFormFileSize">cgiFormResultType cgiFormFileSize(
  char *name, int *sizeP)</a>
</strong><br><dd>cgiFormFileSize attempts to retrieve the size, in bytes, of a
  file uploaded by the browser in response to the
  input field of type <code>file</code> specified by the
  <code>name</code> parameter. On success, the size is stored
  to *sizeP, and this function returns 
  <a href="#cgiFormSuccess">cgiFormSuccess</a>. If the form
  field does not exist, this function returns 
  <a href="#cgiFormNotFound">cgiFormNotFound</a>.
  If the form field exists but no file was uploaded, this function
  returns <a href="#cgiFormNotAFile">cgiFormNotAFile</a>.
<br><dt><strong><a name="cgiFormFileContentType">cgiFormResultType cgiFormFileContentType(
  char *name, char *contentType, int max)</a>
</strong><br><dd>cgiFormString attempts to retrieve the content name claimed by the
  user for the specified form input field of type <code>file</code>. 
  <strong>THERE IS NO GUARANTEE THAT THE CONTENT TYPE WILL BE
  ACCURATE.</strong>
  The content type string will be copied into
  the buffer specified by contentType, up to but not
  exceeding max-1 bytes; a terminating null is then
  added to complete the string. cgiFormFileContentType returns 
  <a href="#cgiFormSuccess">cgiFormSuccess</a> if the string was 
  successfully retrieved and was not empty,
  <a href="#cgiFormNoContentType">cgiFormNoContentType</a> if the string was 
  successfully retrieved but empty indicating that no file was uploaded
  or the browser did not know the content type,
  <a href="#cgiFormTruncated">cgiFormTruncated</a> if the string was
  retrieved but was truncated to fit the buffer,
  and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no 
  such input field was submitted. In the last case, 
  an empty string is copied to result. 

<br><dt><strong><a name="cgiFormFileOpen">cgiFormResultType cgiFormFileOpen(
  char *name, cgiFilePtr *cfpp)</a>
</strong><br><dd>cgiFormFileOpen attempts to open the actual uploaded file data for
  the specified form field of type <code>file</code>. Upon success,
  this function returns retrieve the content name claimed by the
  user for the specified form input field of type <code>file</code>. 
  On success, this function sets *cfpp to a valid cgiFilePtr
  object for use with <a href="#cgiFormFileRead">cgiFormFileRead</a>
  and returns <a href="#cgiFormSuccess">cgiFormSuccess</a>.
  On failure, this function sets *cfpp to a null pointer, and
  returns <a href="#cgiFormNotFound">cgiFormNotFound</a>,
  <a href="#cgiFormNotAFile">cgiFormNotAFile</a>,
  <a href="#cgiFormMemory">cgiFormMemory</a> or
  <a href="#cgiFormIO">cgiFormIO</a> as appropriate.
<p>
  See also <a href="#cgiFormFileRead">cgiFormFileRead</a> 
and <a href="#cgiFormFileClose">cgiFormFileClose</a>.
<br><dt><strong><a name="cgiFormFileRead">cgiFormResultType cgiFormFileRead(
  cgiFilePtr cfp, char *buffer, int bufferSize, int *gotP)</a>
</strong><br><dd>cgiFormFileRead attempts to read up to <code>bufferSize</code>
  bytes from a cgiFilePtr object previously opened with
  <a href="#cgiFormFileOpen">cgiFormFileOpen</a>. If any data
  is successfully read, it is copied to <code>buffer</code>, 
  and the number of bytes successfully read is stored 
  to <code>*gotP</code>. This function returns 
  <a href="#cgiFormSuccess">cgiFormSuccess</a> if any data
  is successfully read. At end of file, this function
  returns <a href="#cgiFormEOF">cgiFormEOF</a>. In the event
  of an I/O error, this function returns 
  <a href="#cgiFormIO">cgiFormIO</a>. If cfp is a null pointer,
  this function returns <a href="#cgiFormOpenFailed">cgiFormOpenFailed</a>.
  <p>
  See also <a href="#cgiFormFileOpen">cgiFormFileOpen</a> 
and <a href="#cgiFormFileClose">cgiFormFileClose</a>.
<br><dt><strong><a name="cgiFormFileClose">cgiFormResultType cgiFormFileClose(
  cgiFilePtr cfp)</a>
</strong><br><dd>cgiFormFileClose closes a cgiFilePtr object previously opened
  with <a href="#cgiFormFileOpen">cgiFormFileOpen</a>, freeing
  memory and other system resources. This
  function returns <a href="#cgiFormSuccess">cgiFormSuccess</a>
  unless cfp is null, in which case 
  <a href="#cgiFormOpenFailed">cgiFormOpenFailed</a> is returned.
<p>
  See also <a href="#cgiFormFileOpen">cgiFormFileOpen</a> 
and <a href="#cgiFormFileRead">cgiFormFileRead</a>.
<br><br><dt><strong><a name="cgiHeaderLocation">
void cgiHeaderLocation(char *redirectUrl)</a>
</strong><br><dd>
cgiHeaderLocation() should be called if the programmer wishes to
redirect the user to a different URL. No further output
is needed in this case.
<p>
If you wish to set cookies,
<strong>you must make your calls to 
<a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a>
and 
<a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a>
</strong> BEFORE invoking cgiHeaderLocation. 
<br><br><dt><strong><a name="cgiHeaderStatus">
void cgiHeaderStatus(int status, char *statusMessage)</a>
</strong><br><dd>
cgiHeaderStatus() should be called if the programmer wishes to
output an HTTP error status code instead of a document. The status
code is the first argument; the second argument is the status
message to be displayed to the user.
<p>
If you wish to set cookies,
<strong>you must make your calls to 
<a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a>
and 
<a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a>
</strong> BEFORE invoking cgiHeaderStatus.
<br><br><dt><strong><a name="cgiHeaderContentType">
void cgiHeaderContentType(char *mimeType)</a>
</strong><br><dd>
cgiHeaderContentType() should be called if the programmer wishes to
output a new document in response to the user's request. This is
the normal case. The single argument is the MIME document type
of the response; typical values are "text/html" for HTML documents, 
"text/plain" for plain ASCII without HTML tags, "image/gif" for
a GIF image and "audio/basic" for .au-format audio.
<p>
If you wish to set cookies,
<strong>you must make your calls to 
<a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a>
and 
<a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a>
</strong> BEFORE invoking cgiHeaderContentType.
<br><br><dt><strong><a name="cgiHeaderCookieSet">
void cgiHeaderCookieSet(char *name, char *value,
  int secondsToLive, char *path, char *domain, int options)</a>
</strong><br><dd>
cgiHeaderCookieSet() should be called when the programmer wishes
to store a piece of information in the user's browser, so that the
stored information is again presented to the server on subsequent
accesses to the relevant site. The first argument is the name of the
cookie to be stored; for best results in all browsers, use a short
name without spaces or unusual punctuation. The second argument is
the value of the cookie to be stored. Again, for best results, use
a short string; it is recommended that cookies be used to store a
unique identifier which is then used to look up more detailed
information in a database on the server side. Attempts to store
elaborate information on the browser side are much more likely to fail.
The third argument is the number of seconds that the cookie should
be kept by the browser; 86400 is a single full day, 365*86400 is
roughly one year. The fourth argument is the partial URL of the
web site within which the cookie is relevant. If the cookie should
be sent to the server for every access to the entire site, 
set this argument to <code>/</code>. The final argument is the
web site name or entire domain for which this cookie should be
submitted; if you choose to have the cookie sent back for an
entire domain, this argument must begin with a dot, such as
<code>.boutell.dev</code>. The cgic variables <a name="#cgiScriptName">cgiScriptName</a>
and <a name="#cgiServerName">cgiServerName</a> are convenient
values for the fourth and fifth arguments.
The sixth argument is a bitmask for specifying cookie security options.
It can be zero (no options) or a bitwise-OR of the following
<code>enum cgiCookieOption</code> values:
<ul>
<li><code>cgiCookieSecure</code></li>
<li><code>cgiCookieHttpOnly</code></li>
<li><code>cgiCookieSameSiteStrict</code></li>
</ul>
See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies">HTTP cookies</a>
for more information.<br>
See also <a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a>,
<a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a>,
<a href="#cgiCookieString">cgiCookieString</a>, 
<a href="#cgiCookieInteger">cgiCookieInteger</a> and
<a href="#cgiCookies">cgiCookies</a>.
<br><br><dt><strong><a name="cgiHeaderCookieSetString">
void cgiHeaderCookieSetString(char *name, char *value,
  int secondsToLive, char *path, char *domain)</a>
</strong><br><dd>
cgiHeaderCookieSetString() is kept for API compatibility reasons. It calls
<a href"#cgiHeaderCookieSet()">cgiHeaderCookieSet</a> with zero (0) as sixth
argument, i.e. no cookie options are set.
<br><br><dt><strong><a name="cgiHeaderCookieSetInteger">
void cgiHeaderCookieSetInteger(char *name, int value,
  int secondsToLive, char *path, char *domain)</a>
</strong><br><dd>
cgiHeaderCookieSetInteger() is identical to
<a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a>,
except that the value to be set is an integer rather than a string.
See <a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a>
for complete information.
<br>
<br><dt><strong><a name="cgiCookieString">cgiFormResultType cgiCookieString(
  char *name, char *result, int max)</a>
</strong><br><dd>cgiFormString attempts to retrieve the string sent for the
  specified cookie (browser-side persistent storage). The 
  text will be copied into
  the buffer specified by result, up to but not
  exceeding max-1 bytes; a terminating null is then
  added to complete the string.
  cgiCookieString returns <a href="#cgiFormSuccess">cgiFormSuccess</a> if the string was 
  successfully retrieved, 
  <a href="#cgiFormTruncated">cgiFormTruncated</a> if the string was
  retrieved but was truncated to fit the buffer,
  cgiFormEmpty if the string was 
  retrieved but was empty, and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no 
  such cookie was submitted. In the last case, 
  an empty string is copied to result. 
<br><br><dt><strong><a name="cgiCookieInteger">cgiFormResultType cgiCookieInteger(
  char *name, int *result, int defaultV)</a>
</strong><br><dd>cgiCookieInteger() attempts to retrieve the integer sent for the
  specified cookie (browser-side persistent storage). The value 
  pointed to by result will be set to the value submitted. 
  cgiCookieInteger() returns 
  cgiFormSuccess if the value was successfully retrieved,
  cgiFormEmpty if the value submitted is an empty string,
  cgiFormBadType if the value submitted is not an integer,
  and <a href="#cgiFormNotFound">cgiFormNotFound</a> if no such 
  input field was submitted. In the last three cases, the value 
  pointed to by result is set to the specified default.<br>
  See also <a href="#cgiCookieString">cgiCookieString</a>,
  <a href="#cgiCookies">cgiCookies</a>,
  <a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a>,
  <a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a>, and
  <a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a>.
<br><br><dt><strong><a name="cgiCookies">cgiFormResultType cgiCookies(
  char *name, char ***ptrToStringArray)</a>
</strong><br><dd>cgiCookies is useful when the programmer cannot know the names
  of all relevant cookies (browser-side persistent strings) in advance.
  The value pointed to by result will be set to a pointer to an array 
  of strings; the last
  entry in the array will be a null pointer.  This array is allocated 
  by the CGI library. Important: when done working with the array,
  you must call cgiStringArrayFree() with the array pointer as the 
  argument. cgiCookies() returns <a href="#cgiFormSuccess">cgiFormSuccess</a> except in the event of an out of memory error.
  On success, ptrToStringArray is set to point to a 
  valid array of strings, with the last element in the array being a 
  null pointer; in the out-of-memory case ptrToStringArray is set to 
  a null pointer, and 
  <a href="#cgiFormOutOfMemory">cgiFormOutOfMemory</a> is returned.
<br><br><dt><strong><a name="cgiHtmlEscape">
cgiFormResultType cgiHtmlEscape(char *s)</a>
</strong><br><dd>
cgiHtmlEscape() outputs the specified null-terminated string to 
<a href="#cgiOut">cgiOut</a>,
escaping any &lt;, &amp;, and &gt; characters encountered correctly so that
they do not interfere with HTML markup. Returns 
<a href="#cgiFormSuccess">cgiFormSuccess</a>, or
<a href="#cgiFormIO">cgiFormIO</a> in the event of an I/O error.
<p> 
<br><br><dt><strong><a name="cgiHtmlEscapeData">
cgiFormResultType cgiHtmlEscapeData(char *data, int len)</a>
</strong><br><dd>
cgiHtmlEscapeData() is identical to <a href="#cgiHtmlEscape">cgiHtmlEscape</a>,
except that the data is not null-terminated. This version of the function
outputs <code>len</code> bytes. See <a href="#cgiHtmlEscape">cgiHtmlEscape</a>
for more information.
<br><br><dt><strong><a name="cgiValueEscape">
cgiFormResultType cgiValueEscape(char *s)</a>
</strong><br><dd>
cgiValueEscape() outputs the specified null-terminated string to 
<a href="#cgiOut">cgiOut</a>,
escaping any " characters encountered correctly so that
they do not interfere with the quotation marks of HTML attribute
values. This is useful when outputting a string as part of the
value attribute of an input tag, or the href attribute of a link
or form tag. This function returns
<a href="#cgiFormSuccess">cgiFormSuccess</a>, or
<a href="#cgiFormIO">cgiFormIO</a> in the event of an I/O error.
<p> 
<br><br><dt><strong><a name="cgiValueEscapeData">
cgiFormResultType cgiValueEscapeData(char *data, int len)</a>
</strong><br><dd>
cgiValueEscapeData() is identical to <a href="#cgiValueEscape">cgiValueEscape</a>,
except that the data is not null-terminated. This version of the function
outputs <code>len</code> bytes. See <a href="#cgiValueEscape">cgiValueEscape</a>
for more information.
<br><br><dt><strong><a name="cgiWriteEnvironment">
cgiEnvironmentResultType cgiWriteEnvironment(char *filename)</a>
</strong><br><dd>
cgiWriteEnvironment() can
  be used to write the entire CGI environment, including
  form data, to the specified output file; <a href="#cgiReadEnvironment">
  cgiReadEnvironment()</a> 
  can then be used to restore that environment from the specified
  input file for debugging. Of course, these will only work as expected
  if you use the <a href="#variables">cgic copies of the CGI environment 
  variables</a> and <a href="#cgiIn">cgiIn</a> and 
  <a href="#cgiOut">cgiOut</a> rather than stdin and
  stdout (also see above). These functions are useful in order 
  to capture real CGI situations while the web server is running, then
  recreate them in a debugging environment. Both functions
  return <a href="#cgiEnvironmentSuccess">cgiEnvironmentSuccess</a> on 
  success, <a href="#cgiEnvironmentIO">cgiEnvironmentIO</a> on an I/O 
  error, and <a href="#cgiEnvironmentMemory">cgiEnvironmentMemory</a>
  on an out-of-memory error.
<br><br><dt><strong><a name="cgiReadEnvironment">
cgiEnvironmentResultType cgiReadEnvironment(char *filename)</a>
</strong><br><dd>
cgiReadEnvironment() restores a CGI environment saved to the specified file by
  <a href="#cgiWriteEnvironment">cgiWriteEnvironment().</a> 
  Of course, these will only work as expected
  if you use the <a href="#variables">cgic copies of the CGI environment 
  variables</a> and <a href="#cgiIn">cgiIn</a> and 
  <a href="#cgiOut">cgiOut</a> rather than stdin and
  stdout (also see above). These functions are useful in order 
  to capture real CGI situations while the web server is running, then
  recreate them in a debugging environment. Both functions
  return <a href="#cgiEnvironmentSuccess">cgiEnvironmentSuccess</a> on success, 
  <a href="#cgiEnvironmentIO">cgiEnvironmentIO</a> on an I/O error, and 
  <a href="#cgiEnvironmentMemory">cgiEnvironmentMemory</a>
  on an out-of-memory error.
<br><br><dt><strong><a name="cgiMain">int cgiMain()</a>
</strong><br><dd><strong>The programmer must write this function</strong>, which performs 
  the unique task of the program and is invoked by the true main() 
  function, found in the cgic library itself. The return value from 
  cgiMain will be the return value of the program. It is expected that 
  the user will make numerous calls to the cgiForm functions
  from within this function. See <a href="#howto">how to write
  a cgic application</a> for details.
</dl>
<h3><a name="variables">cgic variable reference</a></h3>
This section provides a reference guide to the various global
variables provided by cgic for the programmer to utilize.
These variables should always be used in preference to
stdin, stdout, and calls to getenv() in order to ensure
compatibility with the <a href="#debug">cgic CGI debugging features</a>.
<p>
Most of these variables are equivalent to various CGI environment
variables. The most important difference is that the cgic
environment string variables are never null pointers. They will always 
point to valid C strings of zero or more characters.
<dl>
<br><dt><strong><a name="cgiServerSoftware">char *cgiServerSoftware</a>
</strong><br><dd>Points to the name of the server software,
or to an empty string if unknown.
<br><dt><strong><a name="cgiServerName">char *cgiServerName</a>
</strong><br><dd>Points to the name of the server,
or to an empty string if unknown.
<br><dt><strong><a name="cgiGatewayInterface">char *cgiGatewayInterface</a>
</strong><br><dd>Points to the name of the gateway interface (usually CGI/1.1),
or to an empty string if unknown.
<br><dt><strong><a name="cgiServerProtocol">char *cgiServerProtocol</a>
</strong><br><dd>Points to the protocol in use (usually HTTP/1.0),
or to an empty string if unknown.
<br><dt><strong><a name="cgiServerPort">char *cgiServerPort</a>
</strong><br><dd>Points to the port number on which the server is listening
for HTTP connections (usually 80), or an empty string if unknown.
<br><dt><strong><a name="cgiRequestMethod">char *cgiRequestMethod</a>
</strong><br><dd>Points to the method used in the request (usually GET or POST),
or an empty string if unknown (this should not happen).
<br><dt><strong><a name="cgiPathInfo">char *cgiPathInfo</a>
</strong><br><dd>Most web servers recognize any additional path information in 
the URL of the request beyond the name of the CGI program itself and
pass that information on to the program. cgiPathInfo points to this
additional path information.
<br><dt><strong><a name="cgiPathTranslated">char *cgiPathTranslated</a>
</strong><br><dd>Most web servers recognize any additional path information in 
the URL of the request beyond the name of the CGI program itself and
pass that information on to the program. cgiPathTranslated points
to this additional path information, translated by the server into a 
filesystem path on the local server.
<br><dt><strong><a name="cgiScriptName">char *cgiScriptName</a>
</strong><br><dd>Points to the name under which the program was invoked.
<br><dt><strong><a name="cgiQueryString">char *cgiQueryString</a>
</strong><br><dd>Contains any query information submitted by the user as a result
of a GET-method form or an &lt;ISINDEX&gt; tag. Note that this
information need not be parsed directly unless an &lt;ISINDEX&gt; tag
was used; normally it is parsed automatically by the cgic library. Use 
the cgiForm family of functions to retrieve the values associated
with form input fields. See <a href="#howto">how to write
a cgic application</a> for more information.
<br><dt><strong><a name="cgiRemoteHost">char *cgiRemoteHost</a>
</strong><br><dd>Points to the fully resolved hostname of the browser, if known,
or an empty string if unknown.
<br><dt><strong><a name="cgiRemoteAddr">char *cgiRemoteAddr</a>
</strong><br><dd>Points to the dotted-decimal IP address of the browser, if known,
or an empty string if unknown.
<br><dt><strong><a name="cgiAuthType">char *cgiAuthType</a>
</strong><br><dd>Points to the type of authorization used for the request,
if any, or an empty string if none or unknown.
<br><dt><strong><a name="cgiRemoteUser">char *cgiRemoteUser</a>
</strong><br><dd>Points to the user name under which the user has 
authenticated; an empty string if no authentication has
taken place. The certainty of this information depends on
the type of authorization in use; see
<a href="#cgiAuthType">cgiAuthType</a>.
<br><dt><strong><a name="cgiRemoteIdent">char *cgiRemoteIdent</a>
</strong><br><dd>Points to the user name volunteered by the user via
the user identification protocol; an empty
string if unknown. This information is not secure.
Identification demons can be installed by users on
insecure systems such as Windows machines.
<br><dt><strong><a name="cgiContentType">char *cgiContentType</a>
</strong><br><dd>Points to the MIME content type of the information
submitted by the user, if any; an empty string if no
information was submitted. If this string is equal to
<code>application/x-www-form-urlencoded</code> or
<code>multipart/form-data</code>, the cgic
library will automatically examine the form data submitted.
If this string has any other non-empty value, a different
type of data has been submitted. This is currently very rare,
as most browsers can only submit forms and file uploads which
cgic parses directly.
<br><dt><strong><a name="cgiContentType">char *cgiCookie</a>
</strong><br><dd>Points to the raw cookie (browser-side persistent storage)
data submitted by the web browser. 
Programmers should use the functions <a href="#cgiCookies">cgiCookies</a>,
<a href="#cgiCookieString">cgiCookieString</a> and
<a href="#cgiCookieInteger">cgiCookieInteger</a> instead of
examining this string directly.
<br><dt><strong><a name="cgiAccept">char *cgiAccept</a>
</strong><br><dd>Points to a space-separated list of MIME content types
acceptable to the browser (see <a href="#cgiHeaderContentType">
cgiHeaderContentType()</a> ), or an empty string. Unfortunately, this variable
is not supplied in a useful form by most current browsers. Programmers wishing
to make decisions based on the capabilities of the browser
are advised to check the <a href="#cgiUserAgent">cgiUserAgent</a>
variable against a list of browsers and capabilities instead.
<br><dt><strong><a name="cgiUserAgent">char *cgiUserAgent</a>
</strong><br><dd>
Points to the name of the browser in use, or an empty
string if this information is not available. 
<br><dt><strong><a name="cgiReferrer">char *cgiReferrer</a>
</strong><br><dd>
Points to the URL of the previous page visited by the user. This is
often the URL of the form that brought the user to your program.
Note that reporting this information is entirely up to the browser,
which may choose not do so, and may choose not to do so truthfully.
However, this variable is typically accurate. <strong>The frequently
used misspelling cgiReferer is also supplied as a macro.</strong>
<br><dt><strong><a name="cgiContentLength">int cgiContentLength</a>
</strong><br><dd>The number of bytes of form or query data received.
  Note that if the submission is a form or query submission
  the library will read and parse all the information
  directly from cgiIn and/or cgiQueryString. The programmer should
  not do so, and indeed the cgiIn pointer will be at end-of-file
  in such cases.
<br><dt><strong><a name="cgiOut">FILE *cgiOut</a>
</strong><br><dd>Pointer to CGI output. The cgiHeader functions, such as
  <a href="#cgiHeaderContentType">cgiHeaderContentType</a>, should 
  be used first to output the mime headers; the output HTML
  page, GIF image or other web document should then be written
  to cgiOut by the programmer using standard C I/O functions
  such as fprintf() and fwrite(). cgiOut is normally equivalent
  to stdout; however, it is recommended that cgiOut be used to
  ensure compatibility with future versions of cgic for
  specialized environments.
<br><dt><strong><a name="cgiIn">FILE *cgiIn</a>
</strong><br><dd>Pointer to CGI input. In 99.99% of cases, you will not 
  need this. CGIC 2.0 supports both regular POST form submissions
  and multipart/form-data file upload form submissions directly.
</dl>
<H3><a name="resultcodes">cgic result code reference</a></h3>
<p>
In most cases, cgic functions are designed to produce reasonable results
even when browsers and users do unreasonable things. However, it is sometimes
important to know precisely which unreasonable things took place, especially
when assigning a default value or bounding a value is an inadequate
solution. The following result codes are useful in making this determination.
<dl>
<br><dt><strong><a name="cgiFormSuccess">cgiFormSuccess</a>
</strong><br><dd>Indicates that the function successfully performed at least one
action (or retrieved at least one value, where applicable).
<br><dt><strong><a name="cgiFormTruncated">cgiFormTruncated</a>
</strong><br><dd>Indicates that a string value retrieved from the user was
cut short to avoid overwriting the end of a buffer.
<br><dt><strong><a name="cgiFormBadType">cgiFormBadType</a>
</strong><br><dd>Indicates that a "numeric" value submitted by the user was
in fact not a legal number.
<br><dt><strong><a name="cgiFormEmpty">cgiFormEmpty</a>
</strong><br><dd>Indicates that a field was retrieved but contained no data.
<br><dt><strong><a name="cgiFormNotFound">cgiFormNotFound</a>
</strong><br><dd>Indicates that no value was submitted for a particular field.
<br><dt><strong><a name="cgiFormConstrained">cgiFormConstrained</a>
</strong><br><dd>Indicates that a numeric value was beyond the specified bounds
and was forced to the lower or upper bound as appropriate.
<br><dt><strong><a name="cgiFormNoSuchChoice">cgiFormNoSuchChoice</a>
</strong><br><dd>Indicates that the value submitted for a single-choice field
(such as a radio-button group) was not one of the acceptable values.
This usually indicates a discrepancy between the form and the program.
<br><dt><strong><a name="cgiFormEOF">cgiFormEOF</a>
</strong><br><dd>Returned by <a href="#cgiFormFileRead">cgiFormFileRead</a>
when, at the start of the call, the cgiFilePtr object is already
positioned at the end of the uploaded file data. 
<br><dt><strong><a name="cgiFormEOF">cgiFormIO</a>
</strong><br><dd>Returned by <a href="#cgiFormFileRead">cgiFormFileRead</a>
when an I/O error occurs while reading uploaded file data.
<br><dt><strong><a name="cgiFormNotAFile">cgiFormNotAFile</a>
</strong><br><dd>Returned in response to an attempt to manipulate a form field
that is not a file upload field using a file-related function.
<br><dt><strong><a name="cgiFormNoContentType">cgiFormNoContentType</a>
</strong><br><dd>Returned in response to an attempt to fetch the content type of
a file-upload field when the content type is not specified by the browser.
<br><dt><strong><a name="cgiFormNoFileName">cgiFormNoFileName</a>
</strong><br><dd>Returned in response to an attempt to fetch the file name of
a file-upload field when a file name is not specified by the browser.
<br><dt><strong><a name="cgiFormOpenFailed">cgiFormOpenFailed</a>
</strong><br><dd>Returned in response to an attempt to read from a null
cgiFilePtr object, typically when the programmer has failed to
check the result of a call to <a href="#cgiFormFileOpen">cgiFormFileOpen</a>.
<br><dt><strong><a name="cgiEnvironmentMemory">cgiEnvironmentMemory</a>
</strong><br><dd>Indicates that an attempt to read or write the CGI environment
to or from a capture file failed due to an out-of-memory error.
<br><dt><strong><a name="cgiEnvironmentSuccess">cgiEnvironmentSuccess</a>
</strong><br><dd>Indicates that an attempt to read or write the CGI environment
to or from a capture file was successful.
<br><dt><strong><a name="cgiEnvironmentIO">cgiEnvironmentIO</a>
</strong><br><dd>Indicates that an attempt to read or write the CGI environment
to or from a capture file failed due to an I/O error. 
<br><dt><strong><a name="cgiEnvironmentWrongVersion">cgiEnvironmentWrongVersion</a>
</strong><br><dd>Indicates that an attempt to read from a saved debugging CGI environment
produced by a pre-2.0 version of CGIC was made. 
</dl>
<h3><a name="index">cgic quick index</a></h3>
<a href="#cgiAccept">cgiAccept</a> |
<a href="#cgiAuthType">cgiAuthType</a> |
<a href="#cgiContentLength">cgiContentLength</a> |
<a href="#cgiContentType">cgiContentType</a> |
<a href="#cgiEnvironmentIO">cgiEnvironmentIO</a> |
<a href="#cgiEnvironmentMemory">cgiEnvironmentMemory</a> |
<a href="#cgiEnvironmentSuccess">cgiEnvironmentSuccess</a> |
<a href="#cgiCookieInteger">cgiCookieInteger</a> |
<a href="#cgiCookies">cgiCookies</a> |
<a href="#cgiHeaderCookieSet">cgiHeaderCookieSet</a> |
<a href="#cgiHeaderCookieSetString">cgiHeaderCookieSetString</a> |
<a href="#cgiHeaderCookieSetInteger">cgiHeaderCookieSetInteger</a> |
<a href="#cgiCookieString">cgiCookieString</a> |
<a href="#cgiCookieInteger">cgiCookieInteger</a> |
<a href="#cgiHtmlEscape">cgiHtmlEscape</a> |
<a href="#cgiHtmlEscapeData">cgiHtmlEscapeData</a> |
<a href="#cgiValueEscape">cgiValueEscape</a> |
<a href="#cgiValueEscapeData">cgiValueEscapeData</a> |
<a href="#cgiFormBadType">cgiFormBadType</a> |
<a href="#cgiFormCheckboxMultiple">cgiFormCheckboxMultiple()</a> |
<a href="#cgiFormCheckboxSingle">cgiFormCheckboxSingle()</a> |
<a href="#cgiFormConstrained">cgiFormConstrained</a> |
<a href="#cgiFormDouble">cgiFormDouble()</a> |
<a href="#cgiFormDoubleBounded">cgiFormDoubleBounded()</a> |
<a href="#cgiFormEOF">cgiFormEOF</a> |
<a href="#cgiFormEmpty">cgiFormEmpty</a> |
<a href="#cgiFormEntries">cgiFormEntries</a> |
<a href="#cgiFormFileClose">cgiFormFileClose</a> |
<a href="#cgiFormFileContentType">cgiFormFileContentType</a> |
<a href="#cgiFormFileName">cgiFormFileName</a> |
<a href="#cgiFormFileOpen">cgiFormFileOpen</a> |
<a href="#cgiFormFileRead">cgiFormFileRead</a> |
<a href="#cgiFormFileSize">cgiFormFileSize</a> |
<a href="#cgiFormInteger">cgiFormInteger()</a> |
<a href="#cgiFormIntegerBounded">cgiFormIntegerBounded()</a> |
<a href="#cgiFormNoContentType">cgiFormNoContentType</a> |
<a href="#cgiFormNoFileName">cgiFormNoFileName</a> |
<a href="#cgiFormNoSuchChoice">cgiFormNoSuchChoice</a> |
<a href="#cgiFormNotAFile">cgiFormNotAFile</a> |
<a href="#cgiFormNotFound">cgiFormNotFound</a> |
<a href="#cgiFormRadio">cgiFormRadio()</a> |
<a href="#cgiFormSelectMultiple">cgiFormSelectMultiple()</a> |
<a href="#cgiFormSelectSingle">cgiFormSelectSingle()</a> |
<a href="#cgiFormString">cgiFormString()</a> |
<a href="#cgiFormStringMultiple">cgiFormStringMultiple()</a> |
<a href="#cgiFormStringNoNewlines">cgiFormStringNoNewlines()</a> |
<a href="#cgiFormStringSpaceNeeded">cgiFormStringSpaceNeeded()</a> |
<a href="#cgiFormSuccess">cgiFormSuccess</a> |
<a href="#cgiFormTruncated">cgiFormTruncated</a> |
<a href="#cgiGatewayInterface">cgiGatewayInterface</a> |
<a href="#cgiHeaderContentType">cgiHeaderContentType()</a> |
<a href="#cgiHeaderLocation">cgiHeaderLocation()</a> |
<a href="#cgiHeaderStatus">cgiHeaderStatus()</a> |
<a href="#cgiIn">cgiIn</a> |
<a href="#cgiMain">cgiMain()</a>
<a href="#cgiOut">cgiOut</a> |
<a href="#cgiPathInfo">cgiPathInfo</a> |
<a href="#cgiPathTranslated">cgiPathTranslated</a> |
<a href="#cgiQueryString">cgiQueryString</a> |
<a href="#cgiReadEnvironment">cgiReadEnvironment()</a> |
<a href="#cgiReferrer">cgiReferrer()</a> |
<a href="#cgiRemoteAddr">cgiRemoteAddr</a> |
<a href="#cgiRemoteHost">cgiRemoteHost</a> |
<a href="#cgiRemoteIdent">cgiRemoteIdent</a> |
<a href="#cgiRemoteUser">cgiRemoteUser</a> |
<a href="#cgiRequestMethod">cgiRequestMethod</a> |
<a href="#cgiScriptName">cgiScriptName</a> |
<a href="#cgiServerName">cgiServerName</a> |
<a href="#cgiServerPort">cgiServerPort</a> |
<a href="#cgiServerProtocol">cgiServerProtocol</a> |
<a href="#cgiServerSoftware">cgiServerSoftware</a> |
<a href="#cgiStringArrayFree">cgiStringArrayFree()</a> |
<a href="#cgiUserAgent">cgiUserAgent</a> |
<a href="#cgiWriteEnvironment">cgiWriteEnvironment()</a>
