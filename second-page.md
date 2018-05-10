---
layout: page
title:
tags:
---
<body>
<div id="top" class="page" role="document">
  <header role="banner">
    <h1>Second Page</h1>
    <p id="about">This is second test page "SecondPage". First test page is "MainPage". This is a test page filled with common HTML elements. Feel free to practice create your auto-tests.</p>
  </header>

  <fieldset id="redirect__action">
    <legend>Redirect button</legend>
    <p>
      <input id="go_main" type="button" value="Redirect to MainPage" onclick="window.location.href='../main-page'">
    </p>
  </fieldset>

  <nav role="navigation">
    <ul>
      <li>
        <a href="#text">Text</a>
        <ul>
          <li><a href="#text__headings">Headings</a></li>
          <li><a href="#text__paragraphs">Paragraphs</a></li>
          <li><a href="#text__blockquotes">Blockquotes</a></li>
          <li><a href="#text__lists">Lists</a></li>
          <li><a href="#text__hr">Horizontal rules</a></li>
        </ul>
      </li>

  <main role="main">
    <section id="text">
      <header><h1>Text</h1></header>
      <article id="text__headings">
        <header>
          <h1>Headings</h1>
        </header>
        <div>
          <h1>Heading 1</h1>
          <h2>Heading 2</h2>
          <h3>Heading 3</h3>
          <h4>Heading 4</h4>
          <h5>Heading 5</h5>
          <h6>Heading 6</h6>
        </div>
      </article>
      <article id="text__paragraphs">
        <header><h1>Paragraphs</h1></header>
        <div>
          <p>A paragraph (from the Greek paragraphos, “to write beside” or “written beside”) is a self-contained unit of a discourse in writing dealing with a particular point or idea. A paragraph consists of one or more sentences. Though not required by the syntax of any language, paragraphs are usually an expected part of formal writing, used to organize longer prose.</p>
        </div>
      </article>
      <article id="text__blockquotes">
        <header><h1>Blockquotes</h1></header>
        <div>
          <blockquote>
            <p>A block quotation (also known as a long quotation or extract) is a quotation in a written document, that is set off from the main text as a paragraph, or block of text.</p>
            <p>It is typically distinguished visually using indentation and a different typeface or smaller size quotation. It may or may not include a citation, usually placed at the bottom.</p>
            <cite><a href="#!">Said no one, ever.</a></cite>
          </blockquote>
        </div>
      </article>
      <article id="text__lists">
        <header><h1>Lists</h1></header>
        <div>
          <h3>Definition list</h3>
          <dl>
            <dt>Definition List Title</dt>
            <dd>This is a definition list division.</dd>
          </dl>
          <h3>Ordered List</h3>
          <ol>
            <li>List Item 1</li>
            <li>List Item 2</li>
            <li>List Item 3</li>
          </ol>
          <h3>Unordered List</h3>
          <ul>
            <li>List Item 1</li>
            <li>List Item 2</li>
            <li>List Item 3</li>
          </ul>
        </div>
      </article>
      <article id="text__hr">
        <header><h1>Horizontal rules</h1></header>
        <div>
          <hr>
        </div>
        <footer><p><a href="#top">[Top]</a></p></footer>
      </article>
