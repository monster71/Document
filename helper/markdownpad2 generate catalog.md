食用方法：

Tools->Options->Advanced->Custom HTML Head Content->HTML Head Editor->粘贴即可

----------

<script type="text/javascript">
  document.addEventListener("DOMContentLoaded", function() {
        var div1 = document.createElement("div");
        div1.style.cssText = "clear:both";
        // create TOC list
        var outline = document.createElement("div");
        outline.setAttribute("id", "outline-list");
        outline.style.cssText = "min-width:200px;padding:4px 10px;";
        
        var ele_p = document.createElement("p");
        ele_p.style.cssText = "text-align: left; margin: 0;";
        outline.appendChild(ele_p);
        
        var ele_span = document.createElement("h3");
        // ele_span.style.cssText = "float: left;";
        var ele_text=document.createTextNode("目录");
        ele_span.appendChild(ele_text);
        
        ele_p.appendChild(ele_span);

        var ele_ol = document.createElement("ol");
        ele_ol.style.cssText = "line-height:160%;";
        ele_ol.setAttribute("id", "outline_ol");
        outline.appendChild(ele_ol);
        var div1 = document.createElement("div");
        div1.style.cssText = "clear:both";

        document.body.insertBefore(outline, document.querySelector('h4'));
        // get all the headlines
        var headers = document.querySelectorAll('h4');
        if (headers.length < 2)
          return;

        // -----
        var old_h = 0, ol_cnt = 0;
        // -----
        
        for (var i = 0; i < headers.length; i++) {
          
          var ele_ols = null;
            // get H* and prepare for the ordered list 
            var header = headers[i];
            //header.setAttribute("id", "t" + i + header.tagName);
            header.setAttribute("id", header.textContent);
            var h = parseInt(header.tagName.substr(1), 10);
            // -----
            if (!old_h){
              old_h = h;
            }
            
            if (h > old_h) {
              ele_ols = document.createElement("dl");
              var ele_Current = ele_ol;
              if(ele_Current && ol_cnt > 0){
                var temp = ol_cnt;
                while(temp > 0){
                  ele_Current = ele_Current.lastChild;
                  temp--;
                }
              }
              ele_Current.lastChild.appendChild(ele_ols);
              ol_cnt++;
            } else if (h < old_h && ol_cnt > 0) {
              if (h == 1) {
                while (ol_cnt > 0) {
                  ol_cnt--;
                }
              } else {
                ele_ols = document.createElement("dl");
                var ele_Current = ele_ol;
                if(ele_Current && ol_cnt > 0){
                  var temp = ol_cnt;
                  while(temp > 1){
                    ele_Current = ele_Current.lastChild;
                    temp--;
                  }
                }
              ele_Current.appendChild(ele_ols);

              ol_cnt--;
            }
            }
            if (h == 1) {
              while (ol_cnt > 0) {
                ol_cnt--;
              }
            }
            old_h = h;
            // -----
            if (ele_ols){
              ele_li = document.createElement("dd")
              ele_ols.appendChild(ele_li);
            } else {
              ele_li = document.createElement("dd")
              ele_ol.appendChild(ele_li);
            }
            
            var a = document.createElement("a");
            // set href for the TOC item 
            //a.setAttribute("href", "#t" + i + header.tagName);
            a.setAttribute("href", "#" + header.textContent);
            // TOC item text
            a.innerHTML = header.textContent;

            
            ele_li.appendChild(a);
          }
          // -----
          while (ol_cnt > 0) {
            ol_cnt--;
          }
          // -----

        });
        
</script>
<style type="text/css">

	h4{
	color:#fff;
	padding: 5px;
	background-color:#2B6695;
	border: 1px solid #fff;
	border-radius: 5px;
	font-size: 24px;
	margin-top: 15px;
	margin-bottom: 15px;
	}

</style>