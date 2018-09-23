# Local-jsfiddle
Mini Js Fiddle for local machine. It supports:
- [x] Support Shortcuts:  `F5`: Run; `Ctrl+U`: Delete from cursor to head
- [x] Support Toggle auto run
- [x] Support Copy rendered page and editor
- [x] Support open new page
- [x] Support load js

Add the following special url into your browser's bookmark:

    data:text/html,<head></head><body oninput="autorun.checked && run()"> <style> textarea,iframe{width:49%;height:48%} body{margin:0} textarea{width:49%;font-size:18} </style> <textarea placeholder="HTML" id="h"></textarea> <textarea placeholder="CSS" id="c"></textarea> <div> <select id="cdn"> <option>loadjs</option><option>dexie</option><option>jquery</option><option>loadjs</option><option>dexie</option><option>jquery</option></select> <input type="checkbox" id="autorun" checked=""> AutoRun <button onclick="run()">Run</button> <button onclick="open_page('page')">Open Page</button> <button onclick="copy_html('page')">Copy Page</button> <button onclick="copy_html('editor')">Copy Editor</button> <span style="color:red" id="msg"></span> F5: Run; Ctrl+U:Delete Head; MacOSX Tips: Ctrl+A:Head, Ctrl+E:End, Ctrl+F:Right, Ctrl+B:Left, Ctrl+N:Next Line, Ctrl+P: Previous Line </div> <textarea placeholder="JS" id="j"></textarea> <iframe id="i" srcdoc=""></iframe> <script> let cdnjs = { 'dexie': 'https://unpkg.com/dexie@latest/dist/dexie.js', 'jquery': 'https://unpkg.com/dexie@latest/dist/dexie.js', }; cdn.innerHTML=`<option>loadjs</option>`; for(let k in cdnjs){ cdn.innerHTML+=`<option>${k}</option>`; } cdn.onchange = function(){ let src = cdnjs[this.value]; if(src){ h.value = `<script src="${src}"></`+`script>\n`+ h.value; } }; document.addEventListener('keydown', e=>{ if (e.keyCode === 9 && e.target.tagName === 'TEXTAREA') { var target = e.target; var start = target.selectionStart; var end = target.selectionEnd; target.value=target.value.slice(0, start) + "\t" + target.value.slice(end); target.selectionStart = target.selectionEnd = start + 1; e.preventDefault(); }else if(e.key==='F5'){ run(); }else if(e.ctrlKey && e.keyCode===85 && e.target.tagName === 'TEXTAREA') { var target = e.target; var start = target.selectionStart; var end = target.selectionEnd; var start = target.value.slice(0, start).lastIndexOf('\n')+1; target.value=target.value.slice(0, start) + target.value.slice(end); target.selectionStart = target.selectionEnd = start; e.preventDefault(); } }); function run(){ i.srcdoc=h.value+'<style>'+c.value+'</style><script>'+j.value+'<\/script>'; } function open_page(type){ let h = `${i.srcdoc}`; console.log(h); let page = window.open(); page.document.write(h); } function copy_html(type){ if(type === 'page'){ text = `${i.srcdoc}`; } else if(type=== 'editor'){ let srcdoc = i.srcdoc; i.srcdoc = ''; for(let node of [h,c,j]){ node.innerText = node.value; } text = `data:text/html,${document.documentElement.innerHTML}`; i.srcdoc = srcdoc; } if(copy(text)){ msg.innerText = `copy ${type} success!`; setTimeout(v=>msg.innerText='',2000); } } function copy(text) { var node = document.createElement('textarea'); node.innerHTML = text; document.body.appendChild(node); node.select(); var result = document.execCommand('copy'); document.body.removeChild(node); return result; } run(); </script> </body>

More:
- The source code [](fiddle.html)
- The clock demo [](https://rawgit.com/ahuigo/local-jsfiddle/master/fiddle-clock.html)

## Inspired by
- https://github.com/umpox/TinyEditor
