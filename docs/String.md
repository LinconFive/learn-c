### Main

[C2][Main](index.md)😃.

### Emoji
Nowadays we have so many fun chars other than words, as below,
```
# cat f.c
#include <stdio.h>
#include <wchar.h>
#include <locale.h>

#define N 6

int main() {
  setlocale(LC_ALL, "en_US.utf-8");

  // Emoji array. Look up on emojipedia!
  wchar_t emojis[N] = {0x1f332, 0x1f341, 0x1f4cd, 0x1f342, 0x1f698, 0x1f387};

  // Print emoji array
  printf("This could be the description of any inspirational insta picture\n");
  printf("#reasontoroam #nrthwst\n");
  for(int i = 0; i < N; ++i) printf("%lc", emojis[i]);
  printf("\n");
}
root@hwsrv-894377:~# ./a.out 
This could be the description of any inspirational insta picture
#reasontoroam #nrthwst
🌲🍁📍🍂🚘🎇
```

The character developed in half century, 

| phase | coding  | note   |
|-------|---------|--------|
| 1     | ASCII   |        |
| 2     | ANSI    |        |
| 3     | GB2312  |        |
| 4     | GBK     |        |
| 5     | Unicode |        |
| 6     | UTF-16  |        |
| 7     | UTF-8   |        |

### UTF-8
The [UTF-8](https://www.fileformat.info/info/charset/UTF-8/list.htm) can be divided into several groups, as below

- Standard 0-255 Set
- Greek Characters
- General Punctuation
- Superscripts and Subscripts
- Currency Symbols
- Letterlike
- Fractions, Roman Numerals, etc.
- Arrows
- Geometric Shapes
- Block Symbols
- Math Operators and Symbols
- Control Pictures
- Enclosed Alphanumerics
- Box Drawing
- Block Elements
- Geometric Shapes
- Misc. Symbols
- Dingbats
- Misc. Symbols and Arrows
- Emoticons (U+1F600 to U+1F64F)
- Pictographs (Emojis) (U+1F300 to U+1F5FF)
- Transportation/Map
- Musical Symbols
- Domino Tiles
- Playing Cards
- Braille

Let's try to print a sun,
```
# cat g.c
#include <stdio.h>
#include <wchar.h>
#include <locale.h>

int main() {
  setlocale(LC_ALL, "en_US.utf-8");

  // Emoji array. Look up on emojipedia!
  wchar_t emojis = 127774; //\U0001F31E

  // Print emoji array
  printf("%lc\n", emojis);
}
# ./a.out 
🌞
```

### ASCII
Total number of Character in ASCII is 256 (0 to 255).
0 to 31(total 32 character) is called as ASCII control characters.
32 to 127 character is called as ASCII printable characters.
128 to 255 is called as The extended ASCII codes.

|Decimal|Octal|Hex|Binary|Value|Description|
|--- |--- |--- |--- |--- |--- |
|000|000|00|0000 0000|NUL|"null" character|
|001|001|01|0000 0001|SOH|start of header|
|002|002|02|0000 0010|STX|start of text|
|003|003|03|0000 0011|ETX|end of text|
|004|004|04|0000 0100|EOT|end of transmission|
|005|005|05|0000 0101|ENQ|enquiry|
|006|006|06|0000 0110|ACK|acknowledgment|
|007|007|07|0000 0111|BEL|bell|
|008|010|08|0000 1000|BS|backspace|
|009|011|09|0000 1001|HT|horizontal tab|
|010|012|0A|0000 1010|LF|line feed|
|011|013|0B|0000 1011|VT|vertical tab|
|012|014|0C|0000 1100|FF|form feed|
|013|015|0D|0000 1101|CR|carriage return|
|014|016|0E|0000 1110|SO|shift out|
|015|017|0F|0000 1111|SI|shift in|
|016|020|10|0001 0000|DLE|data link escape|
|017|021|11|0001 0001|DC1|device control 1 (XON)|
|018|022|12|0001 0010|DC2|device control 2|
|019|023|13|0001 0011|DC3|device control 3 (XOFF)|
|020|024|14|0001 0100|DC4|device control 4|
|021|025|15|0001 0101|NAK|negative acknowledgement|
|022|026|16|0001 0110|SYN|synchronous idle|
|023|027|17|0001 0111|ETB|end of transmission block|
|024|030|18|0001 1000|CAN|cancel|
|025|031|19|0001 1001|EM|end of medium|
|026|032|1A|0001 1010|SUB|substitute|
|027|033|1B|0001 1011|ESC|escape|
|028|034|1C|0001 1100|FS|file separator|
|029|035|1D|0001 1101|GS|group separator|
|030|036|1E|0001 1110|RS|request to send/record separator|
|031|037|1F|0001 1111|US|unit separator|
|032|040|20|0010 0000|SP|space|
|033|041|21|0010 0001|!|exclamation mark|
|034|042|22|0010 0010|"|double quote|
|035|043|23|0010 0011|#|number sign|
|036|044|24|0010 0100|$|dollar sign|
|037|045|25|0010 0101|%|percent|
|038|046|26|0010 0110|&|ampersand|
|039|047|27|0010 0111|'|single quote|
|040|050|28|0010 1000|(|left/opening parenthesis|
|041|051|29|0010 1001|)|right/closing parenthesis|
|042|052|2A|0010 1010|*|asterisk|
|043|053|2B|0010 1011|+|plus|
|044|054|2C|0010 1100|,|comma|
|045|055|2D|0010 1101|-|minus or dash|
|046|056|2E|0010 1110|.|dot|
|047|057|2F|0010 1111|/|forward slash|
|048|060|30|0011 0000|0||
|049|061|31|0011 0001|1||
|050|062|32|0011 0010|2||
|051|063|33|0011 0011|3||
|052|064|34|0011 0100|4||
|053|065|35|0011 0101|5||
|054|066|36|0011 0110|6||
|055|067|37|0011 0111|7||
|056|070|38|0011 1000|8||
|057|071|39|0011 1001|9||
|058|072|3A|0011 1010|:|colon|
|059|073|3B|0011 1011|;|semi-colon|
|060|074|3C|0011 1100|<|less than|
|061|075|3D|0011 1101|=|equal sign|
|062|076|3E|0011 1110|>|greater than|
|063|077|3F|0011 1111|?|question mark|
|064|100|40|0100 0000|@|"at" symbol|
|065|101|41|0100 0001|A||
|066|102|42|0100 0010|B||
|067|103|43|0100 0011|C||
|068|104|44|0100 0100|D||
|069|105|45|0100 0101|E||
|070|106|46|0100 0110|F||
|071|107|47|0100 0111|G||
|072|110|48|0100 1000|H||
|073|111|49|0100 1001|I||
|074|112|4A|0100 1010|J||
|075|113|4B|0100 1011|K||
|076|114|4C|0100 1100|L||
|077|115|4D|0100 1101|M||
|078|116|4E|0100 1110|N||
|079|117|4F|0100 1111|O||
|080|120|50|0101 0000|P||
|081|121|51|0101 0001|Q||
|082|122|52|0101 0010|R||
|083|123|53|0101 0011|S||
|084|124|54|0101 0100|T||
|085|125|55|0101 0101|U||
|086|126|56|0101 0110|V||
|087|127|57|0101 0111|W||
|088|130|58|0101 1000|X||
|089|131|59|0101 1001|Y||
|090|132|5A|0101 1010|Z||
|091|133|5B|0101 1011|[|left/opening bracket|
|092|134|5C|0101 1100|\|back slash|
|093|135|5D|0101 1101|]|right/closing bracket|
|094|136|5E|0101 1110|^|caret/circumflex|
|095|137|5F|0101 1111|_|underscore|
|096|140|60|0110 0000|`||
|097|141|61|0110 0001|a||
|098|142|62|0110 0010|b||
|099|143|63|0110 0011|c||
|100|144|64|0110 0100|d||
|101|145|65|0110 0101|e||
|102|146|66|0110 0110|f||
|103|147|67|0110 0111|g||
|104|150|68|0110 1000|h||
|105|151|69|0110 1001|i||
|106|152|6A|0110 1010|j||
|107|153|6B|0110 1011|k||
|108|154|6C|0110 1100|l||
|109|155|6D|0110 1101|m||
|110|156|6E|0110 1110|n||
|111|157|6F|0110 1111|o||
|112|160|70|0111 0000|p||
|113|161|71|0111 0001|q||
|114|162|72|0111 0010|r||
|115|163|73|0111 0011|s||
|116|164|74|0111 0100|t||
|117|165|75|0111 0101|u||
|118|166|76|0111 0110|v||
|119|167|77|0111 0111|w||
|120|170|78|0111 1000|x||
|121|171|79|0111 1001|y||
|122|172|7A|0111 1010|z||
|123|173|7B|0111 1011|{|left/opening brace|
|124|174|7C|0111 1100|||vertical bar|
|125|175|7D|0111 1101|}|right/closing brace|
|126|176|7E|0111 1110|~|tilde|
|127|177|7F|0111 1111|DEL|delete|

```
# cat a.c
#include <stdio.h>
int main( void )
{
        unsigned char char1 , char2 , char3 , char4 , char5 , char6 , char7 , char8 ;
        printf("|-------------------------------------------------------------------------------------------------------|\n" );
        printf("|                     ASCII table - excluding control characters                                        |\n" );
        printf("|-------------------------------------------------------------------------------------------------------|\n" );
        printf("|   Ch Dec  Hex  |  Ch Dec  Hex   | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex |\n" );
        printf("|----------------|----------------|-------------|-------------|-------------|-------------|-------------|\n" );
        for( int i = 0 ; i < 32; i++)
        {
                char1 = i;
                char2 = i+32;
                char3 = i+64;
                char4 = i+96;
                char5 = i+128; // extended ASCII characters
                char6 = i+160;
                char7 = i+192;
                char8 = i+224;
                printf( "|  %c    %3d %#x " ,
                                char2 , char2 , char2 );
                printf( "|  %c    %3d %#x " ,
                                char3 , char3 , char3 );
                if( char4 == 127 ) {
                        printf("|%s %3d %#x |" ,
                                        "DEL" , char4 , char4  );
                } else {
                        printf("|  %c %3d %#x |" ,
                                        char4 , char4 , char4  );
                }
                // Print extended ASCII for current system.
                printf("  %c %3d %#x |  %c %3d %#x |  %c %3d %#x |  %c %3d %#x |" ,
                                char5 , char5 , char5 ,
                                char6 , char6 , char6 ,
                                char7 , char7 , char7 ,
                                char8 , char8 , char8  );
                printf( "\n" );
        }
        printf("|-------------------------------------------------------------------------------------------------------|\n" );
}
# ./a.out
|-------------------------------------------------------------------------------------------------------|
|                     ASCII table - excluding control characters                                        |
|-------------------------------------------------------------------------------------------------------|
|   Ch Dec  Hex  |  Ch Dec  Hex   | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex | Ch Dec  Hex |
|----------------|----------------|-------------|-------------|-------------|-------------|-------------|
|        32 0x20 |  @     64 0x40 |  `  96 0x60 |  � 128 0x80 |  � 160 0xa0 |  � 192 0xc0 |  � 224 0xe0 |
|  !     33 0x21 |  A     65 0x41 |  a  97 0x61 |  � 129 0x81 |  � 161 0xa1 |  � 193 0xc1 |  � 225 0xe1 |
|  "     34 0x22 |  B     66 0x42 |  b  98 0x62 |  � 130 0x82 |  � 162 0xa2 |  � 194 0xc2 |  � 226 0xe2 |
|  #     35 0x23 |  C     67 0x43 |  c  99 0x63 |  � 131 0x83 |  � 163 0xa3 |  � 195 0xc3 |  � 227 0xe3 |
|  $     36 0x24 |  D     68 0x44 |  d 100 0x64 |  � 132 0x84 |  � 164 0xa4 |  � 196 0xc4 |  � 228 0xe4 |
|  %     37 0x25 |  E     69 0x45 |  e 101 0x65 |  � 133 0x85 |  � 165 0xa5 |  � 197 0xc5 |  � 229 0xe5 |
|  &     38 0x26 |  F     70 0x46 |  f 102 0x66 |  � 134 0x86 |  � 166 0xa6 |  � 198 0xc6 |  � 230 0xe6 |
|  '     39 0x27 |  G     71 0x47 |  g 103 0x67 |  � 135 0x87 |  � 167 0xa7 |  � 199 0xc7 |  � 231 0xe7 |
|  (     40 0x28 |  H     72 0x48 |  h 104 0x68 |  � 136 0x88 |  � 168 0xa8 |  � 200 0xc8 |  � 232 0xe8 |
|  )     41 0x29 |  I     73 0x49 |  i 105 0x69 |  � 137 0x89 |  � 169 0xa9 |  � 201 0xc9 |  � 233 0xe9 |
|  *     42 0x2a |  J     74 0x4a |  j 106 0x6a |  � 138 0x8a |  � 170 0xaa |  � 202 0xca |  � 234 0xea |
|  +     43 0x2b |  K     75 0x4b |  k 107 0x6b |  � 139 0x8b |  � 171 0xab |  � 203 0xcb |  � 235 0xeb |
|  ,     44 0x2c |  L     76 0x4c |  l 108 0x6c |  � 140 0x8c |  � 172 0xac |  � 204 0xcc |  � 236 0xec |
|  -     45 0x2d |  M     77 0x4d |  m 109 0x6d |  � 141 0x8d |  � 173 0xad |  � 205 0xcd |  � 237 0xed |
|  .     46 0x2e |  N     78 0x4e |  n 110 0x6e |  � 142 0x8e |  � 174 0xae |  � 206 0xce |  � 238 0xee |
|  /     47 0x2f |  O     79 0x4f |  o 111 0x6f |  � 143 0x8f |  � 175 0xaf |  � 207 0xcf |  � 239 0xef |
|  0     48 0x30 |  P     80 0x50 |  p 112 0x70 |  � 144 0x90 |  � 176 0xb0 |  � 208 0xd0 |  � 240 0xf0 |
|  1     49 0x31 |  Q     81 0x51 |  q 113 0x71 |  � 145 0x91 |  � 177 0xb1 |  � 209 0xd1 |  � 241 0xf1 |
|  2     50 0x32 |  R     82 0x52 |  r 114 0x72 |  � 146 0x92 |  � 178 0xb2 |  � 210 0xd2 |  � 242 0xf2 |
|  3     51 0x33 |  S     83 0x53 |  s 115 0x73 |  � 147 0x93 |  � 179 0xb3 |  � 211 0xd3 |  � 243 0xf3 |
|  4     52 0x34 |  T     84 0x54 |  t 116 0x74 |  � 148 0x94 |  � 180 0xb4 |  � 212 0xd4 |  � 244 0xf4 |
|  5     53 0x35 |  U     85 0x55 |  u 117 0x75 |  � 149 0x95 |  � 181 0xb5 |  � 213 0xd5 |  � 245 0xf5 |
|  6     54 0x36 |  V     86 0x56 |  v 118 0x76 |  � 150 0x96 |  � 182 0xb6 |  � 214 0xd6 |  � 246 0xf6 |
|  7     55 0x37 |  W     87 0x57 |  w 119 0x77 |  � 151 0x97 |  � 183 0xb7 |  � 215 0xd7 |  � 247 0xf7 |
|  8     56 0x38 |  X     88 0x58 |  x 120 0x78 |  � 152 0x98 |  � 184 0xb8 |  � 216 0xd8 |  � 248 0xf8 |
|  9     57 0x39 |  Y     89 0x59 |  y 121 0x79 |  � 153 0x99 |  � 185 0xb9 |  � 217 0xd9 |  � 249 0xf9 |
|  :     58 0x3a |  Z     90 0x5a |  z 122 0x7a |  � 154 0x9a |  � 186 0xba |  � 218 0xda |  � 250 0xfa |
|  ;     59 0x3b |  [     91 0x5b |  { 123 0x7b |  � 155 0x9b |  � 187 0xbb |  � 219 0xdb |  � 251 0xfb |
|  <     60 0x3c |  \     92 0x5c |  | 124 0x7c |  � 156 0x9c |  � 188 0xbc |  � 220 0xdc |  � 252 0xfc |
|  =     61 0x3d |  ]     93 0x5d |  } 125 0x7d |  � 157 0x9d |  � 189 0xbd |  � 221 0xdd |  � 253 0xfd |
|  >     62 0x3e |  ^     94 0x5e |  ~ 126 0x7e |  � 158 0x9e |  � 190 0xbe |  � 222 0xde |  � 254 0xfe |
|  ?     63 0x3f |  _     95 0x5f |DEL 127 0x7f |  � 159 0x9f |  � 191 0xbf |  � 223 0xdf |  � 255 0xff |
|-------------------------------------------------------------------------------------------------------|
```

Why is extended not shown? It's because the current locale(`locale -c charmap`) not supported. 
```
# cat a.c
#include <stdio.h>
#include <ctype.h>
#include <locale.h>
#include <limits.h>

static void print_char_page(const char *locale, int max)
{
int c;
char *loc;

if (NULL != (loc = setlocale(LC_CTYPE, NULL)) )
{
printf("locale changed from '%s', ", loc);
}

if (NULL != (loc = setlocale(LC_CTYPE, locale)) )
{
printf("to: '%s'\n", loc);
}

printf("+----+-----+---+\n");
printf("|Hex | Dec |Car|\n");
printf("+----+-----+---+\n");
for (c=0; c<max; c++)
{
if ( isprint(c) )
{
printf("| %02x | %3d | %c |\n", c, c, c);
}
}
}

int main(void)
{
/* change env to locale-specific native environment.*/
print_char_page("", UCHAR_MAX);

return 0;
}
# ./a.out
locale changed from 'C', to: 'C.UTF-8'
+----+-----+---+
|Hex | Dec |Car|
+----+-----+---+
| 20 |  32 |   |
| 21 |  33 | ! |
| 22 |  34 | " |
| 23 |  35 | # |
| 24 |  36 | $ |
| 25 |  37 | % |
| 26 |  38 | & |
| 27 |  39 | ' |
| 28 |  40 | ( |
| 29 |  41 | ) |
| 2a |  42 | * |
| 2b |  43 | + |
| 2c |  44 | , |
| 2d |  45 | - |
| 2e |  46 | . |
| 2f |  47 | / |
| 30 |  48 | 0 |
| 31 |  49 | 1 |
| 32 |  50 | 2 |
| 33 |  51 | 3 |
| 34 |  52 | 4 |
| 35 |  53 | 5 |
| 36 |  54 | 6 |
| 37 |  55 | 7 |
| 38 |  56 | 8 |
| 39 |  57 | 9 |
| 3a |  58 | : |
| 3b |  59 | ; |
| 3c |  60 | < |
| 3d |  61 | = |
| 3e |  62 | > |
| 3f |  63 | ? |
| 40 |  64 | @ |
| 41 |  65 | A |
| 42 |  66 | B |
| 43 |  67 | C |
| 44 |  68 | D |
| 45 |  69 | E |
| 46 |  70 | F |
| 47 |  71 | G |
| 48 |  72 | H |
| 49 |  73 | I |
| 4a |  74 | J |
| 4b |  75 | K |
| 4c |  76 | L |
| 4d |  77 | M |
| 4e |  78 | N |
| 4f |  79 | O |
| 50 |  80 | P |
| 51 |  81 | Q |
| 52 |  82 | R |
| 53 |  83 | S |
| 54 |  84 | T |
| 55 |  85 | U |
| 56 |  86 | V |
| 57 |  87 | W |
| 58 |  88 | X |
| 59 |  89 | Y |
| 5a |  90 | Z |
| 5b |  91 | [ |
| 5c |  92 | \ |
| 5d |  93 | ] |
| 5e |  94 | ^ |
| 5f |  95 | _ |
| 60 |  96 | ` |
| 61 |  97 | a |
| 62 |  98 | b |
| 63 |  99 | c |
| 64 | 100 | d |
| 65 | 101 | e |
| 66 | 102 | f |
| 67 | 103 | g |
| 68 | 104 | h |
| 69 | 105 | i |
| 6a | 106 | j |
| 6b | 107 | k |
| 6c | 108 | l |
| 6d | 109 | m |
| 6e | 110 | n |
| 6f | 111 | o |
| 70 | 112 | p |
| 71 | 113 | q |
| 72 | 114 | r |
| 73 | 115 | s |
| 74 | 116 | t |
| 75 | 117 | u |
| 76 | 118 | v |
| 77 | 119 | w |
| 78 | 120 | x |
| 79 | 121 | y |
| 7a | 122 | z |
| 7b | 123 | { |
| 7c | 124 | | |
| 7d | 125 | } |
| 7e | 126 | ~ |
```

From the above, we can not print `128`, and below is a LOCALE(`locale -m charmaps`) table,

|     HEX     |                 Content                 |     |      HEX      |                 Content                 |
| ----------- | --------------------------------------- | --- | ------------- | --------------------------------------- |
| 0020 — 007F | Basic Latin                             |     | 2580 — 259F   | Block Elements                          |
| 00A0 — 00FF | Latin-1 Supplement                      |     | 25A0 — 25FF   | Geometric Shapes                        |
| 0100 — 017F | Latin Extended-A                        |     | 2600 — 26FF   | Miscellaneous Symbols                   |
| 0180 — 024F | Latin Extended-B                        |     | 2700 — 27BF   | Dingbats                                |
| 0250 — 02AF | IPA Extensions                          |     | 27C0 — 27EF   | Miscellaneous Mathematical Symbols-A    |
| 02B0 — 02FF | Spacing Modifier Letters                |     | 27F0 — 27FF   | Supplemental Arrows-A                   |
| 0300 — 036F | Combining Diacritical Marks             |     | 2800 — 28FF   | Braille Patterns                        |
| 0370 — 03FF | Greek and Coptic                        |     | 2900 — 297F   | Supplemental Arrows-B                   |
| 0400 — 04FF | Cyrillic                                |     | 2980 — 29FF   | Miscellaneous Mathematical Symbols-B    |
| 0500 — 052F | Cyrillic Supplementary                  |     | 2A00 — 2AFF   | Supplemental Mathematical Operators     |
| 0530 — 058F | Armenian                                |     | 2B00 — 2BFF   | Miscellaneous Symbols and Arrows        |
| 0590 — 05FF | Hebrew                                  |     | 2E80 — 2EFF   | CJK Radicals Supplement                 |
| 0600 — 06FF | Arabic                                  |     | 2F00 — 2FDF   | Kangxi Radicals                         |
| 0700 — 074F | Syriac                                  |     | 2FF0 — 2FFF   | Ideographic Description Characters      |
| 0780 — 07BF | Thaana                                  |     | 3000 — 303F   | CJK Symbols and Punctuation             |
| 0900 — 097F | Devanagari                              |     | 3040 — 309F   | Hiragana                                |
| 0980 — 09FF | Bengali                                 |     | 30A0 — 30FF   | Katakana                                |
| 0A00 — 0A7F | Gurmukhi                                |     | 3100 — 312F   | Bopomofo                                |
| 0A80 — 0AFF | Gujarati                                |     | 3130 — 318F   | Hangul Compatibility Jamo               |
| 0B00 — 0B7F | Oriya                                   |     | 3190 — 319F   | Kanbun                                  |
| 0B80 — 0BFF | Tamil                                   |     | 31A0 — 31BF   | Bopomofo Extended                       |
| 0C00 — 0C7F | Telugu                                  |     | 31F0 — 31FF   | Katakana Phonetic Extensions            |
| 0C80 — 0CFF | Kannada                                 |     | 3200 — 32FF   | Enclosed CJK Letters and Months         |
| 0D00 — 0D7F | Malayalam                               |     | 3300 — 33FF   | CJK Compatibility                       |
| 0D80 — 0DFF | Sinhala                                 |     | 3400 — 4DBF   | CJK Unified Ideographs Extension A      |
| 0E00 — 0E7F | Thai                                    |     | 4DC0 — 4DFF   | Yijing Hexagram Symbols                 |
| 0E80 — 0EFF | Lao                                     |     | 4E00 — 9FFF   | CJK Unified Ideographs                  |
| 0F00 — 0FFF | Tibetan                                 |     | A000 — A48F   | Yi Syllables                            |
| 1000 — 109F | Myanmar                                 |     | A490 — A4CF   | Yi Radicals                             |
| 10A0 — 10FF | Georgian                                |     | AC00 — D7AF   | Hangul Syllables                        |
| 1100 — 11FF | Hangul Jamo                             |     | D800 — DB7F   | High Surrogates                         |
| 1200 — 137F | Ethiopic                                |     | DB80 — DBFF   | High Private Use Surrogates             |
| 13A0 — 13FF | Cherokee                                |     | DC00 — DFFF   | Low Surrogates                          |
| 1400 — 167F | Unified Canadian Aboriginal Syllabics   |     | E000 — F8FF   | Private Use Area                        |
| 1680 — 169F | Ogham                                   |     | F900 — FAFF   | CJK Compatibility Ideographs            |
| 16A0 — 16FF | Runic                                   |     | FB00 — FB4F   | Alphabetic Presentation Forms           |
| 1700 — 171F | Tagalog                                 |     | FB50 — FDFF   | Arabic Presentation Forms-A             |
| 1720 — 173F | Hanunoo                                 |     | FE00 — FE0F   | Variation Selectors                     |
| 1740 — 175F | Buhid                                   |     | FE20 — FE2F   | Combining Half Marks                    |
| 1760 — 177F | Tagbanwa                                |     | FE30 — FE4F   | CJK Compatibility Forms                 |
| 1780 — 17FF | Khmer                                   |     | FE50 — FE6F   | Small Form Variants                     |
| 1800 — 18AF | Mongolian                               |     | FE70 — FEFF   | Arabic Presentation Forms-B             |
| 1900 — 194F | Limbu                                   |     | FF00 — FFEF   | Halfwidth and Fullwidth Forms           |
| 1950 — 197F | Tai Le                                  |     | FFF0 — FFFF   | Specials                                |
| 19E0 — 19FF | Khmer Symbols                           |     | 10000 — 1007F | Linear B Syllabary                      |
| 1D00 — 1D7F | Phonetic Extensions                     |     | 10080 — 100FF | Linear B Ideograms                      |
| 1E00 — 1EFF | Latin Extended Additional               |     | 10100 — 1013F | Aegean Numbers                          |
| 1F00 — 1FFF | Greek Extended                          |     | 10300 — 1032F | Old Italic                              |
| 2000 — 206F | General Punctuation                     |     | 10330 — 1034F | Gothic                                  |
| 2070 — 209F | Superscripts and Subscripts             |     | 10380 — 1039F | Ugaritic                                |
| 20A0 — 20CF | Currency Symbols                        |     | 10400 — 1044F | Deseret                                 |
| 20D0 — 20FF | Combining Diacritical Marks for Symbols |     | 10450 — 1047F | Shavian                                 |
| 2100 — 214F | Letterlike Symbols                      |     | 10480 — 104AF | Osmanya                                 |
| 2150 — 218F | Number Forms                            |     | 10800 — 1083F | Cypriot Syllabary                       |
| 2190 — 21FF | Arrows                                  |     | 1D000 — 1D0FF | Byzantine Musical Symbols               |
| 2200 — 22FF | Mathematical Operators                  |     | 1D100 — 1D1FF | Musical Symbols                         |
| 2300 — 23FF | Miscellaneous Technical                 |     | 1D300 — 1D35F | Tai Xuan Jing Symbols                   |
| 2400 — 243F | Control Pictures                        |     | 1D400 — 1D7FF | Mathematical Alphanumeric Symbols       |
| 2440 — 245F | Optical Character Recognition           |     | 20000 — 2A6DF | CJK Unified Ideographs Extension B      |
| 2460 — 24FF | Enclosed Alphanumerics                  |     | 2F800 — 2FA1F | CJK Compatibility Ideographs Supplement |
| 2500 — 257F | Box Drawing                             |     | E0000 — E007F | Tags                                    |

So we may want just print `128` in [UTF-8](https://www.compart.com/en/unicode/U+20AC), namely `U+20AC`
```
# cat h.c
#include <stdio.h>

int main() {
        printf("%s\n", "\u20AC");

}
# ./a.out 
€
```

### Print Styles

Here we want to show different ways of print out latin word `ý`, which is *Latin small letter y with acute*.

1. ASCII Extend
```
$ cat a.c
#include <stdio.h>

int main()
{
        printf("%d => %c\n", 253, 253);
        return 0;
}

$ gcc a.c
$ ./a.out
253 => �
```
2. UTF-16 (hex)
```
#include <stdio.h>
#include <wchar.h>
#include <locale.h>

int main() {
    setlocale(LC_CTYPE, "");
    wchar_t star = 0x00FD;;
    wprintf(L"%lc\n", star);
}
```
3. UTF-8 (hex)
```
#include <stdio.h>

int main() {
        printf("%c%c\n", '\x00', '\xFD');

}
```
4. UTF-16 (unicode)
```
#include <stdio.h>

int main() {
        printf("%s\n", "\u00FD");

}
```

### Format

We need neat print out, so let's learn `%[-][0][width]C`, as below

|  format   |   content  |
| --- | --- |  
| %   | marks the beginning of a format code. |
| –   | optional, specifies left justification of the output argument. |
| 0   | optional, adds zero-padding on the left to match the width. |
| _width_ | an optional width specification. Width specifications and default values are format-code specific, and are described with the format codes below. |
| _C_ | is the format code. See below. |
| %%  | Insert a single % character. |
| %b, %B<br><br>%_w_b, %_w_B<br><br>%_w.m_b, %_w.m_B | Transfer data in binary notation, _w_ is the total width, _m_ is the minimum number of non-blank digits. There is no difference between the lowercase and uppercase forms. |
| %d, %D<br><br>%_w_d, %_w_D<br><br>%_w.m_d, %_w.m_D<br><br>%i, %I<br><br>%_w_i, %_w_I<br><br>%_w.m_i, %_w.m_I | Transfer integer data, _w_ is the total width, _m_ is the minimum number of non-blank digits. There is no difference between the lowercase and uppercase forms. The %d forms are identical to the %i forms and are provided for programmers familiar with C's printf. |
| %e, %E<br><br>%_w_e, %_w_E<br><br>%_w.d_e, %_w.d_E | Transfer data in exponential notation, _w_ is the total width, _d_ is the number of digits after the decimal. Either a lowercase "e" or uppercase "E" will be used depending upon the code. |
| %f, %F<br><br>%_w_f, %_w_F<br><br>%_w.d_f, %_w.d_F | Transfer data in floating-point notation, _w_ is the total width, _d_ is the number of digits after the decimal. There is no difference between the lowercase and uppercase forms. |
| %g, %G<br><br>%_w_g, %_w_G<br><br>%_w.d_g, %_w.d_G | Transfer data in either floating-point or exponential notation, _w_ is the total width, _d_ is the total number of significant digits. Use %e or %E if exponent is less than –4 or greater than or equal to the precision, otherwise use %f. |
| %o, %O<br><br>%_w_o, %_w_O<br><br>%_w.m_o, %_w.m_O | Transfer data in octal notation, _w_ is the total width, _m_ is the minimum number of non-blank digits. There is no difference between the lowercase and uppercase forms. |
| %s, %S<br><br>%_w_s, %_w_S | Transfer character data until either a null byte (\\0) or the total width _w_ is reached. |
| %x, %X<br><br>%_w_x, %_w_X<br><br>%_w.m_x, %_w.m_X<br><br>%z, %Z<br><br>%_w_z, %_w_Z<br><br>%_w.m_z, %_w.m_Z | Transfer data in hexadecimal notation, _w_ is the total width, _m_ is the minimum number of non-blank digits. The lowercase %x or %z will give lowercase hexadecimal digits, while the uppercase %X or %Z will give uppercase. |
| \n |  prints a newline |
| \b |	prints a backspace (backs up one character) |
| \t |	prints a tab character |
| \\ |	prints a backslash |
| \" |	prints a double quote |

```
#include <stdio.h>
#include <locale.h>

#ifndef __STDC_ISO_10646__
#error "Oops, our wide chars are not Unicode codepoints, sorry!"
#endif
int main()
{
        int i;
        setlocale(LC_ALL, "");
        printf("__________________\n");
        for (i = 0x4DC0; i < 0x4DC0 + 64; i++) {
                printf("| %2d | %lc | %04x |\n", i - 0x4DC0 + 1, i, i - 0x4DC0);
        }
        printf("------------------\n");
        return 0;
}
```

<a href="#top">Back to top</a>
