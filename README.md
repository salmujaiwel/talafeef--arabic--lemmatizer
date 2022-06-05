# talafeef--arabic--lemmatizer

# تلافيف
## ما يتضمنه نظام المُعجّم العربي
**أهم ما تمّ عمله حتى الآن**
* منهج تأصيل معجمي موحد يُبقي القسم الكلامي للكلمة (التوكن) على قسمه النحوي دون تغيير
* تركنا التشكيل على الكلمات tokens مرةً، ثم أزلناها مرةً أخرى. ظهرت نتائج نماذج الكلمات إلى متجه بدون الشكيل أفضل.
* إمكانية تحسين نموذج الشَّبكات العصبِيَّة المتكَرّرة بشبكات الذَّاكرة القصيرةِ-الطَّويلةِ المدى بدلا من نموذج الحقول العشوائية المشروطة
* إمكانية تحسين نموذج حقيبية الكلمات المستمرة بدلا من تخطي الكلمة، وذلك بزيادة حجم البيانات 
* استخراج الكلمات ذات العلاقة بنموذج حقيبة الكلمات المستمرة
* استخراج الكلمات سياقيا والمتشابة/المتقابة وفق السياق النصي بنموذج بيرت 
* اعتمدنا على نموذج مدرب مسبقا لبيرت، لاحظنا أن في هذا النموذج المدرّب مسبقا مشكلة في نظام التفريق الذي قام على تفريق المباني الصرفية مع المباني النحوية. سنعمل على تحسين ذلك ببناء نظام جديد للتفريق يقوم على تفريق المباني النحوية فقط، ثم سنعمل على بناء نموذج بيرت بناء عليه، وعلى بياناتنا الخاصة.

# النماذج على الترتيب مكتوبة بالإنجليزية بغية الوضوح (كل هذه النماذج موضوعة في مجلد باسم: نماذج نظام المعجم Arabic Lemmatizer Models)
(Mdels and data can be reached here https://drive.google.com/drive/folders/1xisUw45BkYNVAF6NdfhD4CBnmc0kgo8y?usp=sharing) 
1. CRF Model: (crf_model.sav)
2. RNN (LSTM) Model: (rnn-model.h5)
3. N-Gram Model: (tag2index.pkl and word2index.pkl)
4. Skip-Gram Model: (model.pt)
5. CBOW Model: (model.h5 and embeddings.npz)
6. Pretrained BERT Model (bert-model.h5)
7. BERT: (tokenized_text.npz) and (token_vecs_cat_array.npz) and (tokenized_text.pkl). All are integrated into the GUI. We created the mdoel bert-model.h5 first, then we created a list of embeddings to use it at this stage. 

### N.B. To execute the user interface, kindly run the code source file named as nlpiffy_gui.py in the the folder named as NLPiffy_GUI.

## فكرة النموذج

بُني هذا النموذج بشكل كامل، بدءًا من جمع المدونة العربية، وصولًا إلى بناء معجّم آلي كامل. يُمكن زيادة نصوص مجالات الأوعية الخمسة التي حُدّدت في هذا المشروع، وقد طوّرنا الأدوات والأساليب التي تُساعد على تفريق وتقطيع النصوص آليا، ثم العمل يدويا على (1) استخراج الأصل المعجمي، (2) والمعنى الدلالي الصحيح، (3) والعلاقة الدلالية، (4) والوسم النحوي، (5) والتلازم اللغوي، (6) والتصاحب اللفظي، (7) والتعبير الاصطلاحي، (8) والتنبؤ بالمتلازم والمتصاحب والتعبير الاصطلاحي، (9) وتصحيح المتلازم والمتصاحب والتعبير الاصطلاحي.

## معلومات عن المدونة (البيانات اللغوية)

-	Standard--Academic (A):	10,512 tokens (before tokenizing); 10,557 tokens (after tokenizing); 10,612 tokens (after segmenting)
-	Standard-- Khutbah (K):	10,380 tokens (before tokenizing); 10,713 tokens (after tokenizing); 13,396 tokens (after segmenting)
-	Standard--Journalism (N): 10,408 tokens (before tokenizing); 11,019 tokens (after tokenizing); 12,584 13,396 tokens (after segmenting)
-	Standard--Official-Issues (O): 10,093 tokens (before tokenizing); 10,657 tokens (after tokenizing); 12,351 tokens (after segmenting)
-	Standard--Web (W): 10,097 tokens (before tokenizing); 10,706 tokens (after tokenizing); 12,365 tokens (after segmenting)

**The whole data (Talafeef.txt): 51,490 tokens (before tokenizing); 53,752 tokens (after tokenizing); 61,308 (61,312 IDs as in Talafeef.txt) tokens (after segmenting)**

لماذا هذه الأوعية تحديدًا؟ تزخر اللغة العربية كغيرها من اللغات الرسمية العالمية بتنوع هائل من الأوعية، مع قلة المجالات والموضوعات. حصرنا هذه الأوعية في خمسة فقط، نظرًا لشموليتها. من المعلوم أن المجالات والموضوعات العلمية كالطب مثلا قليلة باللغة العربية، ولكن يُمكن معالجة هذه القلة باستراتيجية تقليل الأوعية لصالح تكثير/توزين تلك المجالات القليلة. هذه الاستراتيجية مقترحه من رئيس الفريق، وستُساعد على زيادة مدونتنا مستقبلا، وفي هذه الزيادة اعتدال قد يتحقق. كيف؟ لو جئنا بأوعية عديدة، فإن الزيادة في المتوفر منها بكثرة قد يُضعف المواد المعجمية في المدونة، مثل: الصحف، التي تكثر فيها الكلمات نفسها، المتوسطة التكرار، على اختلاف جغرافياتها وموضوعاتها. قادنا هذا الافتراض إلى أن نحدد أهم الأوعية التي تشمل مجالات اللغة بوصفها لغةً نموذجية تتمثل فيها مستويات الفصاحة والتفصيح، وهذه الأوعية هي: الوعاء الأكاديمي، والوعاء الديني، والوعاء الصحفي، ووعاء الإصدارات الرسمية، ووعاء الويب.

يُمكن لأي مجال من مجالات النصوص اللغوية العربية أن تدخل نصوصها اللغوية ضمن هذه الخمسة التي حدّدناها، كما سيساعد تحديد هذه الأوعية الخمسة فقط على الانتقائية المتوازنة للنصوص اللغوية العربية. مثلا: يُمكن أن نضيف نصوص الكتب التعليميةالرسمية ضمن وعاء (الإصدارات الرسمية)، ويمكن أن نضيف نصوص الرسائل الجامعية ضمن وعاء النصوص الأكاديمية، ويُمكن أن نضيف نصوص الحوارات ضمن الصحافة وكل ما يرتبط بمنشورات الصحافة والميديا الجديدة من صحف ووكالات أنباء وحوارات تلفزيونية وتواصل اجتماعي ضمن صناعة الصحافة journalism.

## نظام التقطيع والتوسيم النحوي

للعربية خصائص كتابية مركبة، وللتقطيع مناهج معقدة بعض الشيء. التفريق والتقطيع متقاربان في التطبيق، فهما يعتمدان أساسا على فكر الفصل بين الكلمات وغير الكلمات فصلًا يجعل بينهما مسافة واحدة. التفريق يقوم على فصل الكلمات عن غير الكلمات، كفصل علامة الترقيم عن الكلمة، مثل: (محمد، وخالد) التي تُصبح ([محمد]؛ [،]؛ [وخالد])، أو فصل الرموز عن الأرقام، مثل: (23/رمضان/1443)، فتُصبح تتابعيا على نحو ([23]؛ [/]؛ [رمضان]؛ [/]؛ [1443]). أما التقطيع فيقوم على فصل الكلمة وفقا لمبانيها النحوية. هناك بعض الأنظمة التي قامت على فصل الكلمة وفق المباني النحوية والصرفية معا، وفي ذلك إشكال؛ لأن المباني الصرفية جزء لا يتجزأ من شكل الكلمة السياقية، وفي فصلها إشكال. ومثال المباني الصرفية فصل (أل) التعريف مثل: (العلم) فتصيح ([ال]؛ [علم])، أو فصل الضمير المتصل مثل (مؤمنون) فتصبح ([مؤمن]؛ [ون]). إن وجود [ون] لوحدها لا تقيم أي فائدة للذكاء الاصطناعي وفهمه لسلوك الكلمات في النصوص اللغوية. أما المباني النحوية فهي الدقيقة التي بها يقوم التقطيع، مثل: حروف العطف المتصلة: الواو والفاء، مثل: (ومحمد) فتصبح ([و]؛ [محمد]).وفي التوسيم، فإن الوسوم على النحو الآتي:  

1. N: الأسماء؛ كل الأسماء الشائعة والأعلام والكيانات والمصادر والمصدر الصناعي والأعداد المكتوبة بالحروف (واحد، اثنان، ثلاثة، إلخ) والأسماء المنسوبة واسم الآلة واسم المكان واسم الزمان 
2. V: الأفعال؛ (الماضي والمضارع والأمر)
-	VBD: فعل ماضي
-	VBN: فعل مبني للمجهول
-	VBP: فعل مضارع
-	VB: فعل أمر
3. JJ: صفة (المشتقات الآتية فقط): اسم الفاعل، واسم المفعول، وصيغ المبالغة، والصفات المشبهة
4. JJR:	صفة (التفضيل)؛ اسم التفضيل (مثل: أكبر، الأكبر) والألوان على صيغة (أفعل-فعلاء) 
5. RB:	الظروف؛ كل ظروف الزمان والمكان مثل: [ثَمَ]؛ [عند]؛ [بين]؛ إلخ
6. DT:	أسماء الإشارة
7. WP:	الأسماء الموصولة؛ كل أخوات (الذي) الموصولية بالإضافة إلى (ما) عندما تدل على الموصولية سياقيا
8. IN:	حروف الجر؛ كل حروف الجر في النحو العربي ومن ضمنها أيضا [حتى] إلا إذا جاء بعدها جملة فعلية فحينها يكون وسمها أداة
9. CC:	حروف العطف؛ [و]؛ [ف]؛ [ثم]؛ [بل]؛ [أم]
10. PRP: الضمائر المنفصلة؛ [هم]؛ [هما]؛ [هو]؛ [هي]؛ [أنا]؛ [نحن]؛ [أنتم] إلخ.
11. RP:	الأدوات؛ أدوات النصب للأسماء وأدوات النصب والجزم للأفعال والحروف وما قيس على الحروف مثل حرف الجواب (نعم) و(لا) ومثل حرف الزجر (كلا)
12. WRB: أسماء الاستفهام؛ كل أسماء الاستفهام المعروفة، التي تعبر عن الاستفهام. بعض الأدوات تجيء شرطية أو موصولية، وقد وُضعت ضمن صنف الأدوات، وبعضها تجيء ظرفية وقد وضعت ضمن صنف الظروف
13. ABBREV: الاختصارات العربية؛ التي تتعلق بالحرف العربي فقط أما الحرف الأجنبي فقد صُنّف ضمن الكلمات الأجنبية
14. PUNC: علامات الترقيم 
15. CD:	الأعداد	[1]؛ [2]؛ [3]؛ إلخ 
16. FW:	الكلمات الأجنبية بحروف غير عربية
## منهج التوسيم النحوي
1.	كل الأسماء الشائعة وأسماء الأعلام والمصادر الصناعية والأسماء المنسوبة إلى ياء النسب قد جعلت أسماء. 
2.	جعل الصفات على نوعين فقط: JJ للمشتقات (أسماء الفاعل، وأسماء المفعول، وصيغ المبالغة، والصفات المشبهة)، وJJR للصفات المعرفة للمقارنة (الأقل، الأوسط، الأكبر) والألوان: (الأبيض، الأصفر، وصفات المقارنة (أفضل، أكبر، أثقل).
3.	صيغ المبالغة والصفات المشبهة من المشتقات، وهي صفات. جاءت صيغ المبالغة على الأوزان القياسية الآتية: فعّال: صوّام/ مِفعال: مِعطاء/ فعول: صبور/ فعيل: كريم/ فَعِل: قَلِق، أو على الأوزان السماعيّة (غير القياسية): فِعّيل: صِدّيق، قِدّيس/ فُعّال: وُضّاء، عٌجّاب/ فُعال: كُبار، عُراض/ فُعَل: عُذر/ مِفْعيل: مِسْكين/ فُعَلة: هُمزة، لُمزة/ فاعول: فاروق/ تِفْعال: تقتال/ فَعّول: قَدّوس/ فَعّيل: كسّيب/ فُعّيل: سُكّيت. وجاءت الصفات المشبهة على الأوزان القياسية الآتية: فَعِل: هذا طفل تَعِب. فَعلة: أصوات نكدة/ أفعَل: أحمر، حمراء/ فعلان: غضبان/ فعيل: كريم / فَعَل: حَسَن/ فَعْل: ضخم/ فُعُل: جُنُب/ فَعِل: خَشِن/ فَعَال: جَبَان/ فُعَال: شُجاع/ فَعْول: وَقور/ أفعل: أشيب/ فيعل: طَيّب/ فيعَل: فيصل؛ صيرف/ فعيل: صفيّ؛ زكيّ؛ عليّ/ فِعل: صِفر؛ رِخو. [هناك تداخل بين زكي وعلي كونها صفات مشبهة وكونها تأتي في الغالب أسماء أعلام]، جعلنا السياق وحده هو الحَكَم في تحديد الاسمية أو الوصفيّة.  
4.	"من عمل صالحا فلنفسه": جعل (من) موصولية (WP) سواء كانت شرطية أو موصولية. أما إن جاءت على الاستفهامية (أتدرون من المفلس؟) فوسمها النحوي الاستفهام (WRB). هناك أدوات تعمل عمل الظروف ولا ترد للاستفهام حسب السياق: (أنى، كيف، متى، أين)، وقد جُعل وسمها النحوي دالا على الظرفية RB. 
5.	
اتبعنا في منهج رد الكلمات إلى أصلها المعجمي على أسلوب مبسّط جدًا، وخالٍ من التعقيدات، أو من الاختلافات النحوية القائمة بين البصريين والكوفيين. وفي الآتي المنهج المُطبّق: 
-	ردّ كل اسم Noun إلى أصله المعجمي المفرد المذكر. 
-	ردّ كل فعل Verb إلى أصله المعجمي الدال على الماضوية (الفعل الماضي المذكر). 
-	رد الصفات (اسم الفاعل، واسم المفعول، وصيغ المبالغة، والصفات المشبهة) إلى الأصل المفرد المذكر. 
-	جعل كل الأدوات Particles كما هي، وهي ما تبقى من الوسوم (الاختصارات العربية، وعلامات الترقيم، والأعداد، والكلمات الأجنبية) كما هي أيضًا.

###### أسلوب التوسيم اليدوي
-	إمكانية التقطيع والتوسيم بأسلوب مبتكر، يعتمد على أساليب منطقية في ملف الإكسل؛ ومنها: 
-	ترقيم التتابعات اللفظية  ترقيمًا تسلسليًّا كما هي في النص ضمن فئة (ID)؛ لأن ذلك يضمن إعادة الترتيب التتابعي كما هو في النص بعد كل إجراء من الإجراءات الفرزية لأغراض التوسيم والتحشية. 
-	بعض الأقسام الكلامية محدودة الأمثلة، مثل: أسماء الإشارة، والأسماء الموصولة، وحروف الجر، وحروف العطف (تم فرزها متسلسلةً لوسمها بسرعة وسهولة)، عدا بعض الكلمات مثل حرف العطف: (ثُمّ) الذي رُوجع للتفريق بينه وبين كلمة (ثَمّ) الظرفية. 

###### أما التوسيم النحوي للأصل المعجمي فقد جعلت على خمسة وسوم كما في القائمة الآتية: 
-	N	الأسماء	Noun
-	V	الأفعال	Verb
-	JJ	صفة (المشتقات الأربعة)	Adjective; JJR	صفة (أفعل التفضيل، والألوان: أخضر، خضراء)	
-	RB	الظروف	Adverb
-	Particles: DT	أسماء الإشارة; WP	الأسماء الموصولة; IN	حروف الجر; CC	حروف العطف; PRP	الضمائر المنفصلة; RP	الأدوات وما قيس على الحروف; WRB	أسماء الاستفهام	
###### يلي ذلك ما تبقى من الوسوم:
-	ABBREV	الاختصارات العربية
-	PUNC	علامات الترقيم
-	CD	الأعداد
-	FW	الكلمات الأجنبية

###### أمثلة من المخرجات
![image](https://user-images.githubusercontent.com/36333755/172040274-5dcb77ae-79b0-4052-8e89-7c89bd84fc07.png)
###### النتائج الأولية للنماذج
![image](https://user-images.githubusercontent.com/36333755/172040281-491e36f7-8f4f-48e8-84de-0c819bd77327.png)
