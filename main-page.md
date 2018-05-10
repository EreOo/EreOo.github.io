---
layout: page
title:
tags:
---
<div id="top" class="page" role="document">
     <header role="banner">
       <h1>Main Page</h1>
       <p class="about-text" id="about">This is fist test page "MainPage". Second test page is "SecondPage". This is a test page filled with common HTML elements. Feel free to practice create your auto-tests.</p>
     </header>

     <fieldset id="redirect__action">
       <legend>Redirect button</legend>
       <p>
         <input id="go_second" type="button" value="Redirect to TextPage" onclick="window.location.href='../Second-page'">
       </p>
     </fieldset>


         <article id="text__tables">
           <header><h1>Tabular data</h1></header>
           <table>
             <caption>Table Caption</caption>
             <thead>
               <tr>
                 <th>Table Heading 1</th>
                 <th>Table Heading 2</th>
                 <th>Table Heading 3</th>
                 <th>Table Heading 4</th>
                 <th>Table Heading 5</th>
               </tr>
             </thead>
             <tfoot>
               <tr>
                 <th>Table Footer 1</th>
                 <th>Table Footer 2</th>
                 <th>Table Footer 3</th>
                 <th>Table Footer 4</th>
                 <th>Table Footer 5</th>
               </tr>
             </tfoot>
             <tbody>
               <tr>
                 <td>Table Cell 1</td>
                 <td>Table Cell 2</td>
                 <td>Table Cell 3</td>
                 <td>Table Cell 4</td>
                 <td>Table Cell 5</td>
               </tr>
               <tr>
                 <td>Table Cell 1</td>
                 <td>Table Cell 2</td>
                 <td>Table Cell 3</td>
                 <td>Table Cell 4</td>
                 <td>Table Cell 5</td>
               </tr>
               <tr>
                 <td>Table Cell 1</td>
                 <td>Table Cell 2</td>
                 <td>Table Cell 3</td>
                 <td>Table Cell 4</td>
                 <td>Table Cell 5</td>
               </tr>
               <tr>
                 <td>Table Cell 1</td>
                 <td>Table Cell 2</td>
                 <td>Table Cell 3</td>
                 <td>Table Cell 4</td>
                 <td>Table Cell 5</td>
               </tr>
             </tbody>
           </table>
         </article>

         <article id="embedded__iframe">
           <header><h2>IFrame</h2></header>
           <div><iframe src="index.html" height="300"></iframe></div>
         </article>


       <section id="forms">
         <header><h1>Form elements</h1></header>
         <form>
           <fieldset id="forms_input">
             <legend>Input fields</legend>
             <p>
               <label class="label_name" for="input__text">Name</label>
               <input id="input__name" type="text" placeholder="Name Input">
             </p>
             <p>
               <label class="label_password" for="input__password">Password</label>
               <input id="input__password" type="password" placeholder="Type your Password">
             </p>
             <p>
               <label class="label_webadress" for="input__webaddress">Web Address</label>
               <input id="input__webaddress" type="url" placeholder="http://yoursite.com">
             </p>
             <p>
               <label class="label_email" for="input__emailaddress">Email Address</label>
               <input id="input__emailaddress" type="email" placeholder="name@email.com">
             </p>
             <p>
               <label class="label_phone" for="input__phone">Phone Number</label>
               <input id="input__phone" type="tel" placeholder="(999) 999-9999">
             </p>
             <p>
               <label class="label_search" for="input__search">Search</label>
               <input id="input__search" type="search" placeholder="Enter Search Term">
             </p>
             <p>
               <label for="input__text2">Number Input</label>
               <input id="input__text2" type="number" placeholder="Enter a Number">
             </p>
             <p>
               <label for="input__text3" class="error">Error</label>
               <input id="input__text3" class="is-error" type="text" placeholder="Text Input">
             </p>
             <p>
               <label for="input__text4" class="valid">Valid</label>
               <input id="input__text4" class="is-valid" type="text" placeholder="Text Input">
             </p>
           </fieldset>


           <fieldset id="forms__select">
             <legend>Select menus</legend>
             <p>
               <label for="select">Select</label>
               <select id="select">
                 <optgroup label="Option Group">
                   <option>Option One</option>
                   <option>Option Two</option>
                   <option>Option Three</option>
                 </optgroup>
               </select>
             </p>
           </fieldset>


           <fieldset id="forms__checkbox">
             <legend>Checkboxes</legend>
             <ul class="list list--bare">
               <li><label for="checkbox1"><input id="checkbox1" name="checkbox" type="checkbox" checked="checked"> Choice A</label></li>
               <li><label for="checkbox2"><input id="checkbox2" name="checkbox" type="checkbox"> Choice B</label></li>
               <li><label for="checkbox3"><input id="checkbox3" name="checkbox" type="checkbox"> Choice C</label></li>
             </ul>
           </fieldset>

           <fieldset id="forms__radio">
             <legend>Radio buttons</legend>
             <ul class="list list--bare">
               <li><label for="radio1"><input id="radio1" name="radio" type="radio" class="radio" checked="checked"> Option 1</label></li>
               <li><label for="radio2"><input id="radio2" name="radio" type="radio" class="radio"> Option 2</label></li>
               <li><label for="radio3"><input id="radio3" name="radio" type="radio" class="radio"> Option 3</label></li>
             </ul>
           </fieldset>

           <fieldset id="forms__textareas">
             <legend>Textareas</legend>
             <p>
               <label for="textarea">Textarea</label>
               <textarea id="textarea" rows="8" cols="48" placeholder="Enter your message here"></textarea>
             </p>
           </fieldset>
           <fieldset id="forms__html5">
             <legend>HTML5 inputs</legend>
             <p>
               <label for="ic">Color input</label>
               <input type="color" id="ic" value="#000000">
             </p>
             <p>
               <label for="in">Number input</label>
               <input type="number" id="in" min="0" max="10" value="5">
             </p>
             <p>
               <label for="ir">Range input</label>
               <input type="range" id="ir" value="10">
             </p>
             <p>
               <label for="idd">Date input</label>
               <input type="date" id="idd" value="1970-01-01">
             </p>
             <p>
               <label for="idm">Month input</label>
               <input type="month" id="idm" value="1970-01">
             </p>
             <p>
               <label for="idw">Week input</label>
               <input type="week" id="idw" value="1970-W01">
             </p>
             <p>
               <label for="idt">Datetime input</label>
               <input type="datetime" id="idt" value="1970-01-01T00:00:00Z">
             </p>
             <p>
               <label for="idtl">Datetime-local input</label>
               <input type="datetime-local" id="idtl" value="1970-01-01T00:00">
             </p>
           </fieldset>


     <p><a href="#top">[Top]</a></p>
         </form>
       </section>
   </div>
