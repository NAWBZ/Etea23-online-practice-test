# Etea23-online-practice-test

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px;margin-bottom:10px}
.stats-bar{display:flex;justify-content:space-between;background:#f8f9fa;padding:10px;border-radius:10px;margin-bottom:15px;font-weight:600;font-size:14px;border:1px solid #eee}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%;margin-top:20px}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
#resName{font-size: 22px; color: #2980b9; font-weight: 600;}
</style>
</head>
<body>

<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 40:00</div>
<div class="stats-bar">
    <span id="qCounter">Question: 1/100</span>
    <span id="attemptedCounter">Attempted: 0/100</span>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resName"></p>
<p id="resScore"></p>
<p id="resTime"></p>
<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>
<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>MUHAMMAD FAIZAN NAWAB</b>
</div>

<script>
const questions = [

    // 1-30: SCIENCE
    {subject:"Science", q:"A mixture is best defined as:", o:["A substance with fixed composition","A chemical combination of substances","A physical combination of two or more substances","A pure substance"], c:2},
    {subject:"Science", q:"Which of the following is a homogeneous mixture?", o:["Sand and water","Oil and water","Air","Soil"], c:2},
    {subject:"Science", q:"The components of a mixture:", o:["Lose their properties","Gain new properties","Retain their original properties","Form new substances"], c:2},
    {subject:"Science", q:"Which method is best for separating wheat grains from husk?", o:["Filtration","Sieving","Evaporation","Distillation"], c:1},
    {subject:"Science", q:"A heterogeneous mixture has:", o:["Uniform composition","Only one component","Non-uniform composition","Fixed melting point"], c:2},
    {subject:"Science", q:"Which of the following is an element?", o:["Water","Air","Iron","Salt"], c:2},
    {subject:"Science", q:"A compound is formed when elements are combined:", o:["Physically","Temporarily","Chemically","Randomly"], c:2},
    {subject:"Science", q:"Which statement is TRUE about compounds?", o:["Components can be separated easily","They have variable composition","They have fixed composition","They retain properties of elements"], c:2},
    {subject:"Science", q:"A pure substance has:", o:["Variable composition","Impurities","Fixed composition","Different components"], c:2},
    {subject:"Science", q:"Which of the following is a pure substance?", o:["Milk","Brass","Distilled water","Soil"], c:2},
    {subject:"Science", q:"An alloy is a mixture of:", o:["Two non-metals","Metal and non-metal","Two or more metals or a metal and non-metal","Only one metal"], c:2},
    {subject:"Science", q:"Brass is an alloy of:", o:["Iron and carbon","Copper and zinc","Copper and tin","Aluminium and iron"], c:1},
    {subject:"Science", q:"One important characteristic of alloys is that they are:", o:["Softer than pure metals","Poor conductors","More resistant to corrosion","Always magnetic"], c:2},
    {subject:"Science", q:"Stainless steel is commonly used because it:", o:["Rusts easily","Is very soft","Resists rusting","Is a pure metal"], c:2},
    {subject:"Science", q:"Which alloy is used in making electric wires?", o:["Steel","Brass","Aluminium alloy","Bronze"], c:2},
    {subject:"Science", q:"Which of the following is an application of mixtures in daily life?", o:["Oxygen gas","Distilled water","Air we breathe","Gold"], c:2},
    {subject:"Science", q:"The process of separating solid particles from a liquid using filter paper is called:", o:["Sieving","Filtration","Evaporation","Distillation"], c:1},
    {subject:"Science", q:"Sieving is used when components differ in:", o:["Color","Solubility","Particle size","Density"], c:2},
    {subject:"Science", q:"Which method is suitable for separating salt from salt solution?", o:["Filtration","Sieving","Evaporation","Sedimentation"], c:2},
    {subject:"Science", q:"Distillation is mainly used to separate:", o:["Insoluble solids from liquids","Two solids","Two miscible liquids","Large solid particles"], c:2},
    {subject:"Science", q:"Chromatography is used to separate substances based on:", o:["Weight","Solubility and movement","Shape","Hardness"], c:1},
    {subject:"Science", q:"Ink can be separated into different colors using:", o:["Filtration","Evaporation","Chromatography","Sieving"], c:2},
    {subject:"Science", q:"A solution is a:", o:["Heterogeneous mixture","Pure substance","Homogeneous mixture","Compound"], c:2},
    {subject:"Science", q:"In a solution, the substance that dissolves is called:", o:["Solvent","Solute","Residue","Mixture"], c:1},
    {subject:"Science", q:"In salt solution, water is the:", o:["Solute","Residue","Solvent","Precipitate"], c:2},
    {subject:"Science", q:"Formation of a solution occurs due to:", o:["Chemical reaction","Physical mixing","Burning","Freezing"], c:1},
    {subject:"Science", q:"Which factor affects the rate of dissolving?", o:["Color of solute","Temperature","Shape of container","Smell"], c:1},
    {subject:"Science", q:"Which of the following cannot form a solution with water?", o:["Sugar","Salt","Sand","Alcohol"], c:2},
    {subject:"Science", q:"Evaporation is a process in which liquid changes into:", o:["Solid","Gas","Plasma","Ice"], c:1},
    {subject:"Science", q:"Separation techniques are based on differences in:", o:["Chemical nature","Physical properties","Atomic number","Molecular formula"], c:1},

    {subject:"Math", q:"A line is best described as a figure that:", o:["Has two end points","Has one end point","Has no end points and extends endlessly","Has fixed length"], c:2},
    {subject:"Math", q:"A line segment differs from a line because a line segment:", o:["Has no direction","Has two fixed end points","Extends infinitely","Has no length"], c:1},
    {subject:"Math", q:"The founder of Geometry is known as:", o:["Pythagoras","Euclid","Archimedes","Newton"], c:1},
    {subject:"Math", q:"Two lines are said to be parallel if they:", o:["Intersect at one point","Are perpendicular","Never meet even when extended","Meet at right angles"], c:2},
    {subject:"Math", q:"The symbol used to represent a line is:", o:["─","↔","→","∠"], c:1},
    {subject:"Math", q:"When two lines intersect at right angles, they are called:", o:["Parallel lines","Oblique lines","Transversal lines","Perpendicular lines"], c:3},
    {subject:"Math", q:"The angle formed between two perpendicular lines is always:", o:["45°","60°","90°","180°"], c:2},
    {subject:"Math", q:"Adjacent angles always have:", o:["Same measure","A common arm and a common vertex","No common side","Equal bisectors"], c:1},
    {subject:"Math", q:"Which pair represents adjacent angles?", o:["Vertically opposite angles","Alternate interior angles","Angles sharing a common side","Corresponding angles"], c:2},
    {subject:"Math", q:"A line that cuts two or more lines at distinct points is called a:", o:["Parallel line","Perpendicular line","Transversal","Ray"], c:2},
    {subject:"Math", q:"When a transversal cuts two parallel lines, alternate interior angles are:", o:["Unequal","Supplementary","Equal","Adjacent"], c:2},
    {subject:"Math", q:"Corresponding angles formed by a transversal with parallel lines are:", o:["Always unequal","Always equal","Vertically opposite","Complementary"], c:1},
    {subject:"Math", q:"If one angle is 70°, its vertically opposite angle will be:", o:["110°","70°","90°","35°"], c:1},
    {subject:"Math", q:"The sum of a linear pair of angles is always:", o:["90°","180°","360°","45°"], c:1},
    {subject:"Math", q:"An angle measuring more than 90° but less than 180° is called:", o:["Acute angle","Right angle","Obtuse angle","Reflex angle"], c:2},
    {subject:"Math", q:"Which instrument is mainly used for constructing angles?", o:["Divider","Ruler","Protractor","Compass"], c:2},
    {subject:"Math", q:"The bisection of an angle divides it into:", o:["Two unequal parts","Three equal parts","Two equal angles","Four right angles"], c:2},
    {subject:"Math", q:"Which tool is essential for angle bisection?", o:["Protractor only","Compass only","Scale only","Compass and scale"], c:3},
    {subject:"Math", q:"Rotational symmetry means a figure:", o:["Looks same after reflection","Looks same after rotation","Has no symmetry","Has only one line of symmetry"], c:1},
    {subject:"Math", q:"A square has rotational symmetry of order:", o:["1","2","3","4"], c:3},
    {subject:"Math", q:"Which of the following has no rotational symmetry other than 360°?", o:["Square","Rectangle","Scalene triangle","Circle"], c:2},
    {subject:"Math", q:"A straight angle measures:", o:["90°","180°","270°","360°"], c:1},
    {subject:"Math", q:"A reflex angle is always:", o:["Less than 90°","Equal to 180°","More than 180°","Exactly 90°"], c:2},
    {subject:"Math", q:"If two angles are complementary, their sum is:", o:["180°","360°","90°","45°"], c:2},
    {subject:"Math", q:"The point where two rays meet to form an angle is called:", o:["Arm","Side","Vertex","Midpoint"], c:2},
    {subject:"Math", q:"An angle formed by two intersecting lines that are not perpendicular is called:", o:["Right angle","Straight angle","Oblique angle","Zero angle"], c:2},
    {subject:"Math", q:"Which angle cannot be constructed using a compass and ruler alone?", o:["60°","90°","45°","20°"], c:3},
    {subject:"Math", q:"When a transversal intersects parallel lines, interior angles on the same side are:", o:["Equal","Vertically opposite","Supplementary","Corresponding"], c:2},
    {subject:"Math", q:"A ray has:", o:["Two end points","No end point","One end point","Fixed length"], c:2},
    {subject:"Math", q:"Geometry mainly deals with the study of:", o:["Numbers","Chemical reactions","Shapes, lines and angles","Motion"], c:2},

    // 61-80: ENGLISH / GENERAL KNOWLEDGE
    {subject:"English", q:"Abdul Sattar Edhi was famous for his work as a:", o:["Scientist","Social worker","Poet","Politician"], c:1},
    {subject:"English", q:"The main aim of Abdul Sattar Edhi’s life was to:", o:["Gain fame","Serve humanity","Write books","Travel the world"], c:1},
    {subject:"English", q:"Mother Teresa devoted her life to helping:", o:["Rich people","Soldiers","Poor and sick people","Scientists"], c:2},
    {subject:"English", q:"Mother Teresa believed that true service should be done with:", o:["Anger","Pride","Love and kindness","Fear"], c:2},
    {subject:"English", q:"Helen Keller became famous despite being:", o:["Poor","Deaf and blind","Uneducated","Weak"], c:1},
    {subject:"English", q:"The biggest achievement of Helen Keller was that she:", o:["Discovered electricity","Learned to speak and write","Became a doctor","Invented machines"], c:1},
    {subject:"English", q:"Helen Keller’s life teaches us the lesson of:", o:["Wealth","Luck","Determination and courage","Power"], c:2},
    {subject:"English", q:"Galileo Galilei is known as the father of:", o:["Chemistry","Biology","Modern science","Geometry"], c:2},
    {subject:"English", q:"Galileo proved that the earth:", o:["Is flat","Does not move","Moves around the sun","Is the center of the universe"], c:2},
    {subject:"English", q:"Galileo faced punishment because he:", o:["Refused to teach","Opposed old beliefs","Was a poor student","Lost his books"], c:1},
    {subject:"English", q:"The poem “Work” by Henry Van Dyke emphasizes:", o:["Resting all day","The value of honest labor","Complaining about life","Wealth without effort"], c:1},
    {subject:"English", q:"According to the poem Work, true happiness comes from:", o:["Sleeping","Working sincerely","Being famous","Doing nothing"], c:1},
    {subject:"English", q:"The poem “The Sun Is Laughing” by Grace Nichols mainly shows the sun as:", o:["Angry","Lazy","Cheerful and lively","Dangerous"], c:2},
    {subject:"English", q:"In The Sun Is Laughing, the sun is described using:", o:["Facts","Simile only","Personification","Alliteration"], c:2},
    {subject:"English", q:"The poem “The Eagle” by Alfred Lord Tennyson describes the eagle as:", o:["Weak and slow","Powerful and lonely","Noisy and playful","Friendly to humans"], c:1},
    {subject:"English", q:"The phrase “He clasps the crag with crooked hands” shows the eagle’s:", o:["Fear","Strength and grip","Hunger","Laziness"], c:1},
    {subject:"English", q:"Learning First Aid for Children in School highlights the importance of:", o:["Games","Discipline","Immediate help in emergencies","Punishment"], c:2},
    {subject:"English", q:"First aid should be given:", o:["After a long time","By doctors only","Immediately before medical help arrives","At home only"], c:2},
    {subject:"English", q:"One main purpose of teaching first aid to students is to:", o:["Make them doctors","Increase fear","Save lives in emergencies","Pass exams"], c:2},
    {subject:"English", q:"All the lessons and poems mainly teach us to:", o:["Be rich","Be careless","Develop good character and values","Avoid responsibility"], c:2},

    // 81-90: URDU
    {subject:"Urdu", q:"حمد کے شاعر کون ہیں؟", o:["علامہ اقبال","غنی دہلوی","شبنم جمالی","صوفی تبسم"], c:2},
    {subject:"Urdu", q:"نعت کا موضوع کیا ہے؟", o:["وطن سے محبت","فطرت کا حسن","رسولِ اکرم ﷺ کی شان","علم کی اہمیت"], c:2},
    {subject:"Urdu", q:"ڈرامہ “وہ ہوا پشیمان” کس ادیب نے لکھا؟", o:["ضمیر جعفری","صوفی تبسم","امتیاز علی تاج","تاجور نجیب آبادی"], c:3},
    {subject:"Urdu", q:"نظم “چھوٹی چیونٹی” کے شاعر کون ہیں؟", o:["شبنم جمالی","اسماعیل میرٹھی","غنی دہلوی","صہبا اختر"], c:1},
    {subject:"Urdu", q:"نظم “برسات” کے شاعر کون ہیں؟", o:["اسماعیل میرٹھی","صوفی تبسم","تاجور نجیب آبادی","ضمیر جعفری"], c:2},
    {subject:"Urdu", q:"نظم “ہم وطن بھائیوں، ہم وطن ساتھیو” کس شاعر کی تخلیق ہے؟", o:["حفیظ جالندھری","صوفی تبسم","صہبا اختر","شبنم جمالی"], c:1},
    {subject:"Urdu", q:"سبق “سفر ہو رہا ہے” کے مصنف کون ہیں؟", o:["امتیاز علی تاج","مستنصر حسین تارڑ","ضمیر جعفری","کرشن چندر"], c:2},
    {subject:"Urdu", q:"نظم “میرا دیس” کس شاعر نے لکھی؟", o:["صوفی تبسم","شبنم جمالی","صہبا اختر","غنی دہلوی"], c:2},
    {subject:"Urdu", q:"نظم “علم و آداب کی دولت، مولا مجھے عطا کر” کی شاعرہ کون ہیں؟", o:["شبنم جمالی","صہبا اختر","فہمیدہ ریاض","کشور ناہید"], c:0},
    {subject:"Urdu", q:"سبق “سفر ہو رہا ہے” میں بنیادی پیغام کیا ہے؟", o:["آرام کی اہمیت","سفر کے آداب اور تجربات","دولت کی خواہش","کھیل کود"], c:1},

    // 91-100: ISLAMIAT
    {subject:"Islamiat", q:"توحید کی سب سے بڑی اہمیت کیا ہے؟", o:["انسانوں کی عبادت کو مختلف کرنا","اللہ کی وحدانیت کو تسلیم کرنا","صرف سنتوں کی پیروی کرنا","مذہبی تہوار منانا"], c:1},
    {subject:"Islamiat", q:"توحید کے اثرات میں سب سے اہم کیا ہے؟", o:["انسان میں اخلاق کی بہتری","دولت کی زیادتی","طاقتور بننا","بیماریوں سے بچاؤ"], c:0},
    {subject:"Islamiat", q:"نبوت و رسالت کا بنیادی مقصد کیا ہے؟", o:["دنیاوی دولت حاصل کرنا","اللہ کے پیغام کی تبلیغ کرنا","فوج بنانا","سیاست میں حصہ لینا"], c:1},
    {subject:"Islamiat", q:"ختم نبوت کے مطابق محمد ﷺ کا مرتبہ کیا ہے؟", o:["آخری نبی اور رسول","سب سے بڑا بادشاہ","سب سے بڑا عالم","پہلے نبی"], c:0},
    {subject:"Islamiat", q:"معراج کے دوران حضور ﷺ کون کون سی چیزیں دیکھیں؟", o:["زمین کے صرف پہاڑ","آسمان اور جنت و جہنم","دنیا کی تمام حکومتیں","صرف اپنی امت"], c:1},
    {subject:"Islamiat", q:"حوض کوثر کے بارے میں کیا بیان کیا گیا ہے؟", o:["یہ دریا ہے جو جنت میں ہے","یہ دنیا میں موجود پانی ہے","یہ ایک پہاڑ ہے","یہ مدینے میں واقع چشمہ ہے"], c:0},
    {subject:"Islamiat", q:"مقام محمود کا مرتبہ کیا ہے؟", o:["ایک عام آدمی کی دعا","قیامت کے دن محمد ﷺ کا اعلی مقام","مسجد کی ایک جگہ","ایک صحابی کا نام"], c:1},
    {subject:"Islamiat", q:"شفاعت کبریٰ کب ہوگی؟", o:["دنیا میں","قیامت کے دن نیک لوگوں کے لیے","ہر جمعہ","رمضان کے اختتام پر"], c:1},
    {subject:"Islamiat", q:"نبوت و رسالت کے اثرات میں سے کون سا صحیح ہے؟", o:["انسان کی روحانی ترقی","دنیاوی طاقت کی کمی","دولت میں اضافہ","کھیل کود کا فروغ"], c:0},
    {subject:"Islamiat", q:"توحید اور ختم نبوت کا امت کی زندگی پر کیا اثر ہے؟", o:["اخلاقی تربیت اور ہدایت","صرف عبادت میں مشغول رہنا","دنیاوی طاقت میں اضافہ","مذہبی تہوار منانے کی تعلیم"], c:0}



];

let answers=new Array(questions.length).fill(null);
let index=0,time=2400,startTime,timerInt;

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function updateStats() {
    let attempted = answers.filter(a => a !== null).length;
    document.getElementById("qCounter").innerText = `Question: ${index + 1}/${questions.length}`;
    document.getElementById("attemptedCounter").innerText = `Attempted: ${attempted}/${questions.length}`;
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
if(answers[index] === i) d.classList.add(i===q.c?"correct":"wrong");
d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
updateStats();
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
updateStats();
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";
let score=0,subjectStats={};
questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});
let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=40?"PASSED":"FAILED";
resName.innerText=`Student Name: ${studentName.value}`;
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;
subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>
</body>
</html>
