   SUBROUTINE RENDER.MARKDOWN(MARKDOWN,HTML)
*
   GIT.FILENAME = 'RENDER.MARKDOWN'
   GIT.REPO = 'https://github.com/Krowemoh/TCL-Utilities.git'
*
   EQU TRUE TO 1
   EQU FALSE TO 0
*
   HTML = ''
*
   MARKDOWN = CHANGE(MARKDOWN,CHAR(10),@AM)
   NUMBER.OF.LINES = DCOUNT(MARKDOWN,@AM)
*
   BLOCK = ''
   BLOCK.TYPE = 'PARAGRAPH'
*
   FOR I = 1 TO NUMBER.OF.LINES
      LINE = MARKDOWN<I>
*
      IF LINE = '' OR TRIM(LINE) = '' THEN
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
         BLOCK.TYPE = 'PARAGRAPH'
*
      END ELSE IF LINE[1,3] = '```' THEN
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
*
         BLOCK.TYPE = 'CODE'
*
         LOOP
            I = I + 1
            LINE = MARKDOWN<I>
         UNTIL LINE = '```' OR I > NUMBER.OF.LINES DO
            BLOCK = BLOCK : LINE : CHAR(10)
         REPEAT
*
         GOSUB BUILD.BLOCK
*
      END ELSE IF LINE[1,4] = SPACE(4) THEN
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
*
         BLOCK.TYPE = 'CODE'
*
         BLOCK = BLOCK : LINE[5,9999] : CHAR(10)
*
         LOOP
            I = I + 1
            LINE = MARKDOWN<I>
         UNTIL LINE[1,4] # SPACE(4) OR I > NUMBER.OF.LINES DO
            BLOCK = BLOCK : LINE[5,9999] : CHAR(10)
         REPEAT
*
         GOSUB BUILD.BLOCK
         I = I - 1
*
      END ELSE IF LINE MATCHES "> 0X" THEN
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
*
         BLOCK.TYPE = 'BLOCKQUOTE'
*
         IF LINE[1,2] = '> ' THEN
            LINE = TRIM(LINE[3,9999])
         END
         BLOCK = BLOCK : LINE : CHAR(10)
*
         LOOP
            I = I + 1
            LINE = MARKDOWN<I>
         UNTIL NOT(LINE MATCHES "> 0X") OR I > NUMBER.OF.LINES DO
            IF LINE[1,2] = '> ' THEN
               LINE = TRIM(LINE[3,9999])
            END
            BLOCK = BLOCK : LINE : CHAR(10)
         REPEAT
*
         BLOCK.HTML = ''
         CALL RENDER.MARKDOWN(BLOCK,BLOCK.HTML)
         BLOCK = BLOCK.HTML
         GOSUB BUILD.BLOCK
*
         I = I - 1
*
      END ELSE IF LINE MATCHES "- 0X" OR LINE MATCHES "* 0X" OR LINE MATCHES "1N'.' 0X" THEN
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
*
         IF LINE MATCHES "1N'.' 0X" THEN
            BLOCK.TYPE = 'ORDERED.LIST'
         END ELSE
            BLOCK.TYPE = 'UNORDERED.LIST'
         END
*
         IF LINE[1,2] = '- ' OR LINE[1,2] = '* ' OR LINE MATCHES "1N'.' 0X" THEN
            LINE = TRIM(LINE[3,9999])
         END
         BLOCK = BLOCK : LINE : CHAR(10) : CHAR(10)
*
         LOOP
            I = I + 1
            LINE = MARKDOWN<I>
            NEXT.LINE = MARKDOWN<I+1>
         UNTIL (TRIM(LINE) = '' AND TRIM(NEXT.LINE) = '') OR I > NUMBER.OF.LINES DO
            IF LINE[1,2] = '- ' OR LINE[1,2] = '* ' OR LINE MATCHES "1N'.' 0X" THEN
               LINE = TRIM(LINE[3,9999])
            END
            BLOCK = BLOCK : LINE : CHAR(10) : CHAR(10)
         REPEAT
*
         BLOCK.HTML = ''
         CALL RENDER.MARKDOWN(BLOCK,BLOCK.HTML)
         BLOCK = BLOCK.HTML
         GOSUB BUILD.BLOCK
*
      END ELSE
         GOSUB PARSE.LINE
      END
   NEXT I
*
   IF BLOCK.TYPE = '' THEN
      BLOCK.TYPE = 'PARAGRAPH'
   END
*
   IF BLOCK THEN
      GOSUB BUILD.BLOCK
   END
*
   RETURN
*
*********************  S U B R O U T I N E  *********************
*
PARSE.LINE:NULL
*
   TRIGGER.BLOCK = FALSE
*
   PATTERN = "0X'\'1X0X"
*
* Escape characters
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,4)
*
      LINE =  ONE : '<esc>' : SEQ(TWO) :'</esc>' : THREE
   REPEAT
*
* Header
*
   FOR HDR.CTR = 1 TO 6
      PATTERN = "'" : STR('#',HDR.CTR) : " '0X"
*
      LINE = TRIM(LINE)
*
      LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
         ONE = MATCHFIELD(LINE,PATTERN,1)
         TWO = MATCHFIELD(LINE,PATTERN,2)
*
         LINE =  '<h' : HDR.CTR : '>' : TRIM(TWO) :'</h' : HDR.CTR : '>'
*
         IF BLOCK.TYPE THEN
            GOSUB BUILD.BLOCK
         END
         BLOCK.TYPE = 'HEADER'
      REPEAT
   NEXT HDR.CTR
*
* *** horizontal line
*
   IF LINE = '***' OR LINE = '---' THEN
      LINE = '<hr>'
      IF BLOCK.TYPE THEN
         GOSUB BUILD.BLOCK
      END
      BLOCK.TYPE = 'HORIZONTAL.LINE'
   END
*
* `code`
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<code>' : TWO :'</code>' : THREE
   REPEAT
*
* ***bold and italic**
*
   PATTERN = "0X'***'0X'***'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<strong><em>' : TWO :'</em></strong>' : THREE
   REPEAT
*
* ~~strikethrough~~
*
   PATTERN = "0X'~~'0X'~~'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<s>' : TWO :'</s>' : THREE
   REPEAT
*
* **bold**
*
   PATTERN = "0X'**'0X'**'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<strong>' : TWO :'</strong>' : THREE
   REPEAT
*
* __bold__
*
   PATTERN = "0X'__'0X'__'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<strong>' : TWO :'</strong>' : THREE
   REPEAT
*
* *italic*
*
   PATTERN = "0X'*'0X'*'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<em>' : TWO :'</em>' : THREE
   REPEAT
*
* _italic_
*
   PATTERN = "0X'_'0X'_'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<em>' : TWO :'</em>' : THREE
   REPEAT
*
* ...small...
*
   PATTERN = "0X'...'0X'...'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      LINE =  ONE : '<small>' : TWO :'</small>' : THREE
   REPEAT
*
* ![link](http://link.com)
*
   PATTERN = "0X'!['0X']''('0X')'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,6)
      FOUR = MATCHFIELD(LINE,PATTERN,8)
*
      LINE = ONE : '<img src="' : THREE : '" alt="' : TWO : '">' : FOUR
   REPEAT
*
* [link](http://link.com)
*
   PATTERN = "0X'['0X']''('0X')'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,6)
      FOUR = MATCHFIELD(LINE,PATTERN,8)
*
      LINE =  ONE : '<a href="' : THREE : '">' : TWO :'</a>' : FOUR
   REPEAT
*
* => Gemini Link
*
   PATTERN = "'=> '0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,2)
*
      THREE = TRIM(FIELD(TWO,' ',2,9999))
      TWO = TRIM(FIELD(TWO,' ',1))
*
      IF BLOCK.TYPE THEN
         GOSUB BUILD.BLOCK
      END
*
      EXT = TWO[LEN(TWO)-3,999]
*
      IF EXT = '.png' OR EXT = '.jpg' OR EXT = 'jpeg' OR EXT = '.gif' THEN
         LINE =  '<img src="' : TWO : '" alt="' : THREE :'">'
      END ELSE
         LINE =  '&#8658; <a href="' : TWO : '">' : THREE :'</a>'
      END
*
      BLOCK.TYPE = 'GEMINI.LINK'
   REPEAT
*
* Unescape characters
*
   PATTERN = "0X'<esc>'0X'</esc>'0X"
*
   LOOP UNTIL NOT(LINE MATCHES PATTERN) DO
      ONE = MATCHFIELD(LINE,PATTERN,1)
      TWO = MATCHFIELD(LINE,PATTERN,3)
      THREE = MATCHFIELD(LINE,PATTERN,5)
*
      IF NUM(TWO) THEN
         LINE =  ONE : CHAR(TWO) : THREE
      END ELSE
         LINE = CHANGE(LINE,'<esc>','')
         LINE = CHANGE(LINE,'</esc>','')
      END
   REPEAT
*
   IF BLOCK.TYPE = 'HEADER' OR BLOCK.TYPE = 'HORIZONTAL.LINE' THEN
      BLOCK = LINE
      GOSUB BUILD.BLOCK
   END ELSE
      BLOCK = BLOCK : LINE
   END
*
   RETURN
*
*********************  S U B R O U T I N E  *********************
*
BUILD.BLOCK:NULL
*
   IF BLOCK.TYPE = 'PARAGRAPH' THEN
      IF BLOCK THEN
         HTML = HTML : '<p>' : BLOCK : '</p>' : CHAR(10)
      END
*
   END ELSE IF BLOCK.TYPE = 'HEADER' THEN
      HTML = HTML : BLOCK : CHAR(10)
*
   END ELSE IF BLOCK.TYPE = 'HORIZONTAL.LINE' THEN
      HTML = HTML : BLOCK : CHAR(10)
*
   END ELSE IF BLOCK.TYPE = 'GEMINI.LINK' THEN
      HTML = HTML : '<p>' : BLOCK : '</p>' : CHAR(10)
*
   END ELSE IF BLOCK.TYPE = 'CODE' THEN
      HTML = HTML : '<pre><code>' : BLOCK : '</code></pre>' : CHAR(10)
*
   END ELSE IF BLOCK.TYPE = 'BLOCKQUOTE' THEN
      HTML = HTML : '<blockquote>' : CHAR(10) : BLOCK : '</blockquote>' : CHAR(10)
*
   END ELSE IF BLOCK.TYPE = 'UNORDERED.LIST' OR BLOCK.TYPE = 'ORDERED.LIST' THEN
      IF BLOCK.TYPE = 'UNORDERED.LIST' THEN
         OPEN.LIST.TAG = '<ul>'
         CLOSE.LIST.TAG = '</ul>'
      END ELSE
         OPEN.LIST.TAG = '<ol>'
         CLOSE.LIST.TAG = '</ol>'
      END
*
      HTML = HTML : OPEN.LIST.TAG
      CONVERT CHAR(10) TO @AM IN BLOCK
      NUMBER.OF.ITEMS = DCOUNT(BLOCK,@AM)
      FOR LIST.CTR = 1 TO NUMBER.OF.ITEMS
         LIST.ITEM = BLOCK<LIST.CTR>
         IF LIST.ITEM THEN
            HTML = HTML : CHAR(10) : '<li>' : BLOCK<LIST.CTR> : '</li>'
         END
      NEXT LIST.CTR
      HTML = HTML : CHAR(10) : CLOSE.LIST.TAG : CHAR(10)
*
   END ELSE
      HTML = HTML : '<undefined>' : BLOCK : '</undefined>'
   END
*
   BLOCK = ''
   BLOCK.TYPE = ''
*
   RETURN
*
* END OF PROGRAM
*
   END
*
