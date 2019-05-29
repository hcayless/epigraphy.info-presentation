# Introducing CETEIcean for EpiDoc

If you've used EpiDoc much, you're probably also familiar with the way we usually turn it into something you can read with a web browser: XSLT. This process takes an XML file and outputs HTML, and in the process reformats the input as needed. CETEIcean acheives the same end goal in a different way, one that lets the web browser do the work, using JavaScript and CSS, instead of requiring that you convert the sources ahead of time. 

One way isn't necessarily better than the other. Both JavaScript and XSLT are fully-functional programming languages, and are therefore more-or-less equivalent in what they can do. But their particular strengths may work better in one situation or another. CETEIcean is particularly good in situations where you don't want to either convert all your XML ahead of time or lack the resources to do it on the fly. It also sidesteps some of the issues XSLT has: most programming languages only support the much older and less powerful XSLT 1.0; powerful as it is, it's a bit of a niche language, and you might have an easier time finding someone who can work in JavaScript than XSLT; it can be resource-intensive.

We're going to look at one scenario where CETEIcean works very well: presenting static TEI source documents using GitHub Pages. If you set it up right, you can collaboratively edit a source file on GitHub and then immediately see your changes when you push them, without having to do any extra work. I've chosen at random an example from EDH, <https://edh-www.adw.uni-heidelberg.de/edh/inschrift/HD000102>. The source is available on GitHub at <https://github.com/epigraphic-database-heidelberg/data/blob/master/inscriptions/1/1/HD000102.xml>, and the "raw" version (i.e. just the XML) at <https://raw.githubusercontent.com/epigraphic-database-heidelberg/data/master/inscriptions/1/1/HD000102.xml>.

The simplest way to use CETEIcean is to simply create an HTML file that will link to the JavaScript and CSS you need and then load (using JavaScript) the XML source you want and insert it into the document when a browser loads the HTML page. Unlike XLST, CETEIcean works by directly converting the source file's EpiDoc XML into HTML Custom Elements, so, for example, 

```xml
<origin>
  <origPlace>
    <placeName type="provinceItalicRegion" ref="http://www.trismegistos.org/place/003201">Britannia</placeName>
    <placeName ref="http://www.trismegistos.org/place/003201">Vindolanda</placeName>
  </origPlace>
  <origDate notBefore-custom="0146" datingMethod="http://en.wikipedia.org/wiki/Julian_calendar">146 AD</origDate>
</origin>
```
will get turned into
```html
<tei-origin data-origname="origin" data-processed>
  <tei-origplace data-origname="origPlace" data-processed>
    <tei-placename type="provinceItalicRegion" ref="http://www.trismegistos.org/place/003201" data-origname="placeName" data-processed>Britannia</tei-placename>
    <tei-placename ref="http://www.trismegistos.org/place/003201" data-origname="placeName" data-processed="">Vindolanda</tei-placename> .   </tei-origplace>
  <tei-origdate notbefore-custom="0146" datingmethod="http://en.wikipedia.org/wiki/Julian_calendar" data-origname="origDate" data-processed>146 AD</tei-origdate>
</tei-origin>
```
