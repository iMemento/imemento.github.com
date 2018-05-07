<section class="comment">
<div id="disqus_thread"></div>
<script>

var xhr = new XMLHttpRequest();
xhr.open('GET', '//disqus.com/next/config.json?' + new Date().getTime(), true);
xhr.timeout = 3000;
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
        var disqus_config = function () 
        {
            this.page.url = 'https://peakcoder.com' + '{{ page.url }}'; 
            this.page.identifier = '{{ page.title }}';
        };

        (function() { // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        s.src = 'https://peakcoder.disqus.com/embed.js';
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
        })();
        
    }
}
xhr.ontimeout = function () {
    xhr.abort();
    // do nothing
}
xhr.onerror = function() {
    // do nothing
}
xhr.send(null);
</script>
        
</section>
