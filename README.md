## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/sarahrider/sarahrider.github.io/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Jade Text Comparisons

from difflib import SequenceMatcher
from difflib import Differ
import pandas as pd
from IPython.display import Markdown
import re
Read Excel cheet
df = pd.read_excel("/Users/sarah/Downloads/TEST Jade Text Punctuation.xlsx").fillna("")
New code to make the markdown
markdown_for_all_rows = []

row_counter = 0

for row in df.iterrows():
    
    row_counter += 1
    
    text1 = row[1][1]
    text2 = row[1][2]
    
    if text1 > '' and text2 > '':
    
        markdown_for_row = []
            
        #print()
        #print('text1', text1)
        #print()
        #print('text1', text2)
   
        comparison = SequenceMatcher(None, text1, text2)
        opcodes = comparison.get_opcodes()

        merged_c = []
        
        for s in opcodes:
            if s[0] == 'equal':
                for a in range(s[1], s[2]):
                    merged_c.append([text1[a], ''])
            if s[0] == 'insert':
                for a in range(s[3], s[4]):
                    merged_c.append([text2[a], 'insert'])
            if s[0] == 'delete':
                for a in range(s[1], s[2]):
                    merged_c.append([text1[a], 'delete'])
            if s[0] == 'replace':
                for a in range(s[1], s[2]):
                    merged_c.append([text1[a], 'delete'])
                for a in range(s[3], s[4]):
                    merged_c.append([text2[a], 'insert'])
        
        #print()
        #print('merged_c', merged_c)
                    
        parent_open = False
        for a in range(0, len(merged_c)):
            if merged_c[a][0] == '(':
                parent_open = True
                merged_c[a][1] = 'parenthesis'
            elif merged_c[a][0] == ')':
                parent_open = False
                merged_c[a][1] = 'parenthesis'
            elif parent_open == True:
                merged_c[a][1] = 'parenthesis'
                
        for a in range(0, len(merged_c)):
            if merged_c[a][0] in [' ', '.', ','] and merged_c[a][1] != 'parenthesis':
                merged_c[a][1] = ''
        
        #print()
        #print('merged_c', merged_c)
        
        last_class = None
        
        for c in merged_c:
            
            if c[1] != last_class:
            
                if last_class != None and last_class != '':
                    markdown_for_row.append('</span>')
                
                if c[1].strip() > '':
                    markdown_for_row.append('<span class="' + c[1] + '">')
            
            markdown_for_row.append(c[0])
            last_class = c[1]
            
        if last_class != None and last_class > '':
            markdown_for_row.append('</span>')
            
        #print()
        #print('markdown_for_row', markdown_for_row)
        
        markdown_for_all_rows.append(''.join(markdown_for_row))
            
        print()
        print('markdown_for_all_rows[-1]', markdown_for_all_rows[-1])
markdown_for_all_rows[-1] 玉椀來回部, 輸誠供闔閶, 召公懷不寶, 韓子戒無當,異致白毛鹿, 引恬頳尾魴 <span class="parenthesis">(回部葉爾奇木哈什哈爾初役屬於準噶爾,為所拘縶,因我大軍戡定伊犁,始釋之令歸所部,其長伯克和卓遣使求內屬,此其所貢也)</span>, 勞徠非力并, 天眷奉昭彰.

markdown_for_all_rows[-1] 玉盤博徑得二尺,圍六尺有<span class="parenthesis">(去聲)</span>五寸益.虛中盛水受一石,素質不雕其色碧. 旁達孚尹瓊華澤,葆光撫不留手跡.<span class="delete">群</span><span class="insert">羣</span>玉之精出<span class="delete">昆侖</span><span class="insert">菎崙</span>,吉日甲子天子賓. <span class="delete">於</span><span class="insert">于</span>西王母<span class="delete">瑤</span><span class="insert">瑶</span>池津,行觴 <span class="parenthesis">(I just ,added in a test, parenthesis here)</span> 介紹簠<span class="delete">蓋樽</span><span class="insert">簋罇</span>.爾時所御器今存,作鎮西極永好完. 未入震旦三千年,問今何來不脛偶.準噶爾亡淪世守,阿睦撒納茲竊取. 王師<span class="delete">深</span><span class="insert">罙</span>入靖孽醜,於將獲之<span class="delete">聯</span><span class="insert">聮</span>猭走.棄其重器為我有,元英大呂陳座右. 咄哉玉盤徒華滋,不可食兮不可衣.連城價詎如窮奇,俘彼禍除可罷師 <span class="parenthesis">(and another one was added here)</span>. 前歌後舞樂雍熙,<span class="delete">瑰</span><span class="insert">瓌</span>玩吾將安用之.擬付剿人一例椎.

markdown_for_all_rows[-1] <span class="parenthesis">(did this work?)</span> 于闐何必購奩環,通貢薄來每厚還,.保定不期致<span class="delete">達</span><span class="insert">遠</span>域,琢磨亦復藉他山.示禎碧落星辰表,延喜明廷樽俎<span class="delete">問</span>,<span class="insert">間</span>.漉雪浮香真恰當,思推解渴福區寰. <span class="parenthesis">(maybe?)</span>

markdown_for_all_rows[-1] 會未于闐遣使求,堅昆玉椀自來投,詎云有翼千斤獻,那肯無端大白浮.賢問乃明君子德,工言幾誤楚人璆,歲時宴饗斟香茗,延喜明廷弈葉庥 <span class="parenthesis">(yes!)</span>.
styleblock = """<style>                           
    span.delete {color: #32a852; 
                 background-color: lavender;
                 font-size: 150%; 
                 margin: 0 3px; 
                 border: 1px solid #808080; 
                 line-height: 1.5;
                 padding: 2px;}
    span.insert {color: #e02427;
                 background-color: lavender;
                 font-size: 150%; 
                 margin: 0 3px; 
                 border: 1px solid #808080; 
                 line-height: 1.5;
                 padding: 2px;}
    span.parenthesis {color: #324ea8;
                    font-size: 100%;
                    margin: 0 3px;}
    </style>"""

Markdown(styleblock + '\n\n'.join(markdown_for_all_rows))
玉椀來回部, 輸誠供闔閶, 召公懷不寶, 韓子戒無當,異致白毛鹿, 引恬頳尾魴 (回部葉爾奇木哈什哈爾初役屬於準噶爾,為所拘縶,因我大軍戡定伊犁,始釋之令歸所部,其長伯克和卓遣使求內屬,此其所貢也), 勞徠非力并, 天眷奉昭彰.

玉盤博徑得二尺,圍六尺有(去聲)五寸益.虛中盛水受一石,素質不雕其色碧. 旁達孚尹瓊華澤,葆光撫不留手跡.群羣玉之精出昆侖菎崙,吉日甲子天子賓. 於于西王母瑤瑶池津,行觴 (I just ,added in a test, parenthesis here) 介紹簠蓋樽簋罇.爾時所御器今存,作鎮西極永好完. 未入震旦三千年,問今何來不脛偶.準噶爾亡淪世守,阿睦撒納茲竊取. 王師深罙入靖孽醜,於將獲之聯聮猭走.棄其重器為我有,元英大呂陳座右. 咄哉玉盤徒華滋,不可食兮不可衣.連城價詎如窮奇,俘彼禍除可罷師 (and another one was added here). 前歌後舞樂雍熙,瑰瓌玩吾將安用之.擬付剿人一例椎.

(did this work?) 于闐何必購奩環,通貢薄來每厚還,.保定不期致達遠域,琢磨亦復藉他山.示禎碧落星辰表,延喜明廷樽俎問,間.漉雪浮香真恰當,思推解渴福區寰. (maybe?)

會未于闐遣使求,堅昆玉椀自來投,詎云有翼千斤獻,那肯無端大白浮.賢問乃明君子德,工言幾誤楚人璆,歲時宴饗斟香茗,延喜明廷弈葉庥 (yes!).

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/sarahrider/sarahrider.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
