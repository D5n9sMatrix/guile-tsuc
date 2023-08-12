<!DOCTYPE html>
<!-- saved from url=(0084)https://www.gnu.org/software/emacs/manual/html_node/elisp/Custom-Format-Strings.html -->
<html><!-- Created by GNU Texinfo 7.0.3, https://www.gnu.org/software/texinfo/ --><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<title>Custom Format Strings (GNU Emacs Lisp Reference Manual)</title>

<meta name="description" content="Custom Format Strings (GNU Emacs Lisp Reference Manual)">
<meta name="keywords" content="Custom Format Strings (GNU Emacs Lisp Reference Manual)">
<meta name="resource-type" content="document">
<meta name="distribution" content="global">
<meta name="Generator" content="makeinfo">
<meta name="viewport" content="width=device-width,initial-scale=1">

<link rev="made" href="mailto:bug-gnu-emacs@gnu.org">
<link rel="icon" type="image/png" href="https://www.gnu.org/graphics/gnu-head-mini.png">
<meta name="ICBM" content="42.256233,-71.006581">
<meta name="DC.title" content="gnu.org">
<style type="text/css">
@import url('/software/emacs/manual.css');
</style>
</head>

<body lang="en">
<div class="section-level-extent" id="Custom-Format-Strings">
<div class="nav-panel">
<p>
Next: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Case-Conversion.html" accesskey="n" rel="next">Case Conversion in Lisp</a>, Previous: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Formatting-Strings.html" accesskey="p" rel="prev">Formatting Strings</a>, Up: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Strings-and-Characters.html" accesskey="u" rel="up">Strings and Characters</a> &nbsp; [<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html#SEC_Contents" title="Table of contents" rel="contents">Contents</a>][<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html" title="Index" rel="index">Index</a>]</p>
</div>
<hr>
<h3 class="section" id="Custom-Format-Strings-1">4.8 Custom Format Strings</h3>
<a class="index-entry-id" id="index-custom-format-string"></a>
<a class="index-entry-id" id="index-custom-_0025_002dsequence-in-format"></a>

<p>Sometimes it is useful to allow users and Lisp programs alike to
control how certain text is generated via custom format control
strings.  For example, a format string could control how to display
someone’s forename, surname, and email address.  Using the function
<code class="code">format</code> described in the previous section, the format string
could be something like <code class="code">"%s&nbsp;%s&nbsp;&lt;%s&gt;"</code><!-- /@w -->.  This approach
quickly becomes impractical, however, as it can be unclear which
specification character corresponds to which piece of information.
</p>
<p>A more convenient format string for such cases would be something like
<code class="code">"%f&nbsp;%l&nbsp;&lt;%e&gt;"</code><!-- /@w -->, where each specification character carries
more semantic information and can easily be rearranged relative to
other specification characters, making such format strings more easily
customizable by the user.
</p>
<p>The function <code class="code">format-spec</code> described in this section performs a
similar function to <code class="code">format</code>, except it operates on format
control strings that use arbitrary specification characters.
</p>
<dl class="first-deffn first-defun-alias-first-deffn">
<dt class="deffn defun-alias-deffn" id="index-format_002dspec"><span class="category-def">Function: </span><span><strong class="def-name">format-spec</strong> <var class="def-var-arguments">template spec-alist &amp;optional ignore-missing split</var><a class="copiable-link" href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Custom-Format-Strings.html#index-format_002dspec"> ¶</a></span></dt>
<dd><p>This function returns a string produced from the format string
<var class="var">template</var> according to conversions specified in <var class="var">spec-alist</var>,
which is an alist (see <a class="pxref" href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Association-Lists.html">Association Lists</a>) of the form
<code class="code">(<var class="var">letter</var>&nbsp;.&nbsp;<var class="var">replacement</var>)</code><!-- /@w -->.  Each specification
<code class="code">%<var class="var">letter</var></code> in <var class="var">template</var> will be replaced by
<var class="var">replacement</var> when formatting the resulting string.
</p>
<p>The characters in <var class="var">template</var>, other than the format
specifications, are copied directly into the output, including their
text properties, if any.  Any text properties of the format
specifications are copied to their replacements.
</p>
<p>Using an alist to specify conversions gives rise to some useful
properties:
</p>
<ul class="itemize mark-bullet">
<li>If <var class="var">spec-alist</var> contains more unique <var class="var">letter</var> keys than there
are unique specification characters in <var class="var">template</var>, the unused keys
are simply ignored.
</li><li>If <var class="var">spec-alist</var> contains more than one association with the same
<var class="var">letter</var>, the closest one to the start of the list is used.
</li><li>If <var class="var">template</var> contains the same specification character more than
once, then the same <var class="var">replacement</var> found in <var class="var">spec-alist</var> is
used as a basis for all of that character’s substitutions.
</li><li>The order of specifications in <var class="var">template</var> need not correspond to
the order of associations in <var class="var">spec-alist</var>.
</li></ul>

<p>REPLACEMENT can also be a function taking no arguments, and returning
a string to be used for the replacement.  It will only be called when
the corresponding LETTER is used in the TEMPLATE.  This is useful, for
example, to avoid prompting for input unless it is needed.
</p>
<p>The optional argument <var class="var">ignore-missing</var> indicates how to handle
specification characters in <var class="var">template</var> that are not found in
<var class="var">spec-alist</var>.  If it is <code class="code">nil</code> or omitted, the function
signals an error; if it is <code class="code">ignore</code>, those format specifications
are left verbatim in the output, including their text properties, if
any; if it is <code class="code">delete</code>, those format specifications are removed
from the output; any other non-<code class="code">nil</code> value is handled like
<code class="code">ignore</code>, but any occurrences of ‘<samp class="samp">%%</samp>’ are also left verbatim
in the output.
</p>
<p>If the optional argument <var class="var">split</var> is non-<code class="code">nil</code>, instead of
returning a single string, <code class="code">format-spec</code> will split the result
into a list of strings, based on where the substitutions were
performed.  For instance:
</p>
<div class="example">
<pre class="example-preformatted">(format-spec "foo %b bar" '((?b . "zot")) nil t)
     ⇒ ("foo " "zot" " bar")
</pre></div>
</dd></dl>

<p>The syntax of format specifications accepted by <code class="code">format-spec</code> is
similar, but not identical, to that accepted by <code class="code">format</code>.  In
both cases, a format specification is a sequence of characters
beginning with ‘<samp class="samp">%</samp>’ and ending with an alphabetic letter such as
‘<samp class="samp">s</samp>’.
</p>
<p>Unlike <code class="code">format</code>, which assigns specific meanings to a fixed set
of specification characters, <code class="code">format-spec</code> accepts arbitrary
specification characters and treats them all equally.  For example:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(setq my-site-info
      (list (cons ?s system-name)
            (cons ?t (symbol-name system-type))
            (cons ?c system-configuration)
            (cons ?v emacs-version)
            (cons ?e invocation-name)
            (cons ?p (number-to-string (emacs-pid)))
            (cons ?a user-mail-address)
            (cons ?n user-full-name)))

(format-spec "%e %v (%c)" my-site-info)
     ⇒ "emacs 27.1 (x86_64-pc-linux-gnu)"

(format-spec "%n &lt;%a&gt;" my-site-info)
     ⇒ "Emacs Developers &lt;emacs-devel@gnu.org&gt;"
</pre></div></div>

<p>A format specification can include any number of the following flag
characters immediately after the ‘<samp class="samp">%</samp>’ to modify aspects of the
substitution.
</p>
<dl class="table">
<dt>‘<samp class="samp">0</samp>’</dt>
<dd><p>This flag causes any padding specified by the width to consist of
‘<samp class="samp">0</samp>’ characters instead of spaces.
</p>
</dd>
<dt>‘<samp class="samp">-</samp>’</dt>
<dd><p>This flag causes any padding specified by the width to be inserted on
the right rather than the left.
</p>
</dd>
<dt>‘<samp class="samp">&lt;</samp>’</dt>
<dd><p>This flag causes the substitution to be truncated on the left to the
given width and precision, if specified.
</p>
</dd>
<dt>‘<samp class="samp">&gt;</samp>’</dt>
<dd><p>This flag causes the substitution to be truncated on the right to the
given width, if specified.
</p>
</dd>
<dt>‘<samp class="samp">^</samp>’</dt>
<dd><p>This flag converts the substituted text to upper case (see <a class="pxref" href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Case-Conversion.html">Case Conversion in Lisp</a>).
</p>
</dd>
<dt>‘<samp class="samp">_<span class="r"> (underscore)</span></samp>’</dt>
<dd><p>This flag converts the substituted text to lower case (see <a class="pxref" href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Case-Conversion.html">Case Conversion in Lisp</a>).
</p></dd>
</dl>

<p>The result of using contradictory flags (for instance, both upper and
lower case) is undefined.
</p>
<p>As is the case with <code class="code">format</code>, a format specification can include
a width, which is a decimal number that appears after any flags, and a
precision, which is a decimal-point ‘<samp class="samp">.</samp>’ followed by a decimal
number that appears after any flags and width.
</p>
<p>If a substitution contains fewer characters than its specified width,
it is padded on the left:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(format-spec "%8a is padded on the left with spaces"
             '((?a . "alpha")))
     ⇒ "   alpha is padded on the left with spaces"
</pre></div></div>

<p>If a substitution contains more characters than its specified
precision, it is truncated on the right:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(format-spec "%.2a is truncated on the right"
             '((?a . "alpha")))
     ⇒ "al is truncated on the right"
</pre></div></div>

<p>Here is a more complicated example that combines several
aforementioned features:
</p>
<div class="example">
<div class="group"><pre class="example-preformatted">(setq my-battery-info
      (list (cons ?p "73")      ; Percentage
            (cons ?L "Battery") ; Status
            (cons ?t "2:23")    ; Remaining time
            (cons ?c "24330")   ; Capacity
            (cons ?r "10.6")))  ; Rate of discharge

(format-spec "%&gt;^-3L : %3p%% (%05t left)" my-battery-info)
     ⇒ "BAT :  73% (02:23 left)"

(format-spec "%&gt;^-3L : %3p%% (%05t left)"
             (cons (cons ?L "AC")
                   my-battery-info))
     ⇒ "AC  :  73% (02:23 left)"
</pre></div></div>

<p>As the examples in this section illustrate, <code class="code">format-spec</code> is
often used for selectively formatting an assortment of different
pieces of information.  This is useful in programs that provide
user-customizable format strings, as the user can choose to format
with a regular syntax and in any desired order only a subset of the
information that the program makes available.
</p>
</div>
<hr>
<div class="nav-panel">
<p>
Next: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Case-Conversion.html">Case Conversion in Lisp</a>, Previous: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Formatting-Strings.html">Formatting Strings</a>, Up: <a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Strings-and-Characters.html">Strings and Characters</a> &nbsp; [<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html#SEC_Contents" title="Table of contents" rel="contents">Contents</a>][<a href="https://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html" title="Index" rel="index">Index</a>]</p>
</div>





</body></html>