<html>
<style>
  #scale {
    padding-left: 5px;
  }
</style>
<body style="font-family:'barlow'">
<h2>Scale Writer</h2>
<select id="rootSelect">
  <option value="cSharp">C♯</option>
  <option value="fSharp">F♯</option>
  <option value="bNatural">B</option>
  <option value="eNatural">E</option>
  <option value="aNatural">A</option>
  <option value="dNatural">D</option>
  <option value="gNatural">G</option>
  <option value="cNatural" selected>C</option>
  <option value="fNatural">F</option>
  <option value="bFlat">B♭</option>
  <option value="eFlat">E♭</option>
  <option value="aFlat">A♭</option>
  <option value="dFlat">D♭</option>
  <option value="gFlat">G♭</option>
  <option value="cFlat">C♭</option>  
  </select>
  <select id="chordSelect">
    <option value="0">ma7 (Major)</option>
    <option value="1">ma7 (Major Bebop)</option>
    <option value="2">ma7 (Major Pentatonic)</option>
    <option value="3">ma+4 (Lydian)</option>
    <option value="4">7 (Mixolydian)</option>
    <option value="5">7 (Mixolydian Bebop)</option>
    <option value="6">7 (Major Pentatonic)</option>
    <option value="7">7 (Major Blues)</option>
    <option value="8">7 (Minor Blues)</option>
    <option value="9">7 (Composite Blues)</option>
    <option value="10">7♭9 (3rd Mode of Major Bebop)</option>
    <option value="11">7♯9 (Diminished Whole-Tone)</option>
    <option value="12">7♯11 (Lydian Dominant)</option>
    <option value="13">7+ (Whole-Tone)</option>
    <option value="14">mi7 (Dorian)</option>
    <option value="15">mi7 (Minor Pentatonic)</option>
    <option value="16">mi7 (Minor Blues)</option>
    <option value="17">-7♭5 (Locrian)</option>
    <option value="18">°7 (WH Diminished)</option>
    <option value="19">ma+5 (Lydian Augmented)</option>
    <option value="20">mi(ma7) (Melodic Minor)</option>
    <option value="21">mi(ma♭6) (Harmonic Minor)</option>
    <option value="22">* (Chromatic)</option>     
  </select><br>
<button onclick="scaleFunction()">Add Scale</button>
<button onclick="myBarline()">Add |</button>
<button onclick="myBarlinedbl()">Add ||</button>
<button onclick="myRepeatbar()">Add %</button>
<button onclick="myRepeatbar()">Add %</button>
<button onclick="clearFunction()">Clear</button>
<button onclick="editFunction()">Edit</button>
<button onclick="copyAll()">Copy All</button><br>
<div id="scale" contenteditable="true"></div>
<script>
//nestedArrays//
const rootNested = [[2, 7, 12, 17, 22, 27, 32, 37, 43], [17, 22, 27, 31, 37, 43, 49, 54, 59], [31, 37, 43, 48, 54, 59, 65, 70, 76], [11, 17, 22, 26, 31, 37, 43, 48, 54], [26, 31, 37, 42, 48, 54, 59, 64, 70], [6, 11, 17, 21, 26, 31, 37, 42, 48], [21, 26, 31, 36, 42, 48, 54, 58, 64], [1, 6, 11, 16, 21, 26, 31, 36, 42], [16, 21, 26, 30, 36, 42, 48, 53, 58], [30, 36, 42, 47, 53, 58, 64, 69, 75], [10, 16, 21, 25, 30, 36, 42, 47, 53], [25, 30, 36, 41, 47, 53, 58, 63, 69], [5, 10, 16, 20, 25, 30, 36, 41, 47], [20, 25, 30, 35, 41, 47, 53, 57, 63], [0, 5, 10, 15, 20, 25, 30, 35, 41]];
const degNested = [[0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 4, 5, 6, 7], [0, 1, 2, 4, 5, 7], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 5, 6, 6, 7], [0, 1, 2, 4, 5, 7], [0, 1, 2, 2, 4, 5, 7], [0, 2, 3, 3, 4, 6, 7], [0, 1, 2, 2, 3, 3, 4, 5, 6, 7], [0, 1, 2, 2, 3, 4, 5, 6, 7], [0, 1, 2, 2, 3, 4, 6, 7], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 6, 7], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 2, 3, 4, 6, 7], [0, 2, 3, 3, 4, 6, 7], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 5, 5, 6, 7], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 1, 2, 3, 4, 5, 6, 7, 1], [0, 0, 1, 1, 2, 3, 3, 4, 4, 5, 5, 6, 7]];
const qualNested = [[0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, -1, 0, 0, 0], [0, 0, 0, 0, 0, 0], [0, 0, 0, -1, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 1, 0, 0], [0, 0, 0, 0, 0, 0, 1, 0, 0], [0, 0, 0, 0, 0, 0], [0, 0, 1, 0, 0, 0, 0], [0, 1, 0, -1, 0, 1, 0], [0, 0, 1, 0, 0, -1, 0, 0, 1, 0], [0, 1, 1, 0, 0, 0, 1, 1, 0], [0, 1, 1, 0, -1, -1, 1, 0], [0, 0, 0, -1, 0, 0, 1, 0, 0], [0, 0, 0, -1, -1, 1, 0], [0, 0, 1, 0, 0, 0, 1, 0, 0], [0, 1, 0, 0, 1, 0], [0, 1, 0, -1, 0, 1, 0], [0, 1, 1, 0, 1, 1, 1, 0, 1], [0, 0, 1, 0, 1, 1, 0, 0, 0], [0, 0, 0, -1, -1, 0, 0, 0, 0], [0, 0, 1, 0, 0, 0, 0, 0, 0], [0, 0, 1, 0, 0, 1, 0, 0, 0], [0, -1, 0, -1, 0, 0, -1, 0, -1, 0, -1, 0, 0]];
//copy//
function copyAll() {
  alert("Copied!");
  const richTextDiv = document.getElementById("scale");
  const clipboardItem = new ClipboardItem({
	"text/plain": new Blob(
		[richTextDiv.innerText],
		{ type: "text/plain" }
	),
	"text/html": new Blob(
		[richTextDiv.outerHTML],
		{ type: "text/html" }
	),
});
navigator.clipboard.write([clipboardItem]);
}
//edit//
function editFunction() {
  document.getElementById("scale").focus();
}
//clear//
function clearFunction() {
  let text;
  if (confirm("Are you sure?") == true) {
    document.getElementById("scale").innerHTML = "";
  }
}
//barline//
function myBarline() {
  const div = document.getElementById("scale");
  div.insertAdjacentHTML('beforeend',"<b>𝄖𝄖𝄖𝄖𝄖𝄖𝄖𝄖𝄖𝄖𝄖𝄖</b>" + "<br>");
  }
function myBarlinedbl() {
  const div = document.getElementById("scale");
  div.insertAdjacentHTML('beforeend',"<b>𝄗𝄗𝄗𝄗𝄗𝄗𝄗𝄗𝄗𝄗𝄗𝄗</b>" + "<br>");
  }
function myRepeatbar() {
  const div = document.getElementById("scale");
  div.insertAdjacentHTML('beforeend',"        𝄎        " + "<br>");
  }
function scaleFunction() {
  let a = rootSelect.options[rootSelect.selectedIndex].text;
  let b = chordSelect.options[chordSelect.selectedIndex].text;
  const noteArray = ["C♭","C","C♯","D","C","D♭","D","D♯","E","D","E♭","E","E♯","F♯","E♭","F♭","F","F♯","G","F","G♭","G","G♯","A","G","A♭","A","A♯","B","A","B♭","B","B♯","C♯","B♭","C♭","C","C♯","D","C♭","C","D♭","D","D♯","E","D♭","D","E♭","E","E♯","F♯","E♭","F♭","F","F♯","G","F","G♭","G","G♯","A","G♭","G","A♭","A","A♯","B","A♭","A","B♭","B","B♯","C♯","B♭","C♭","C","C♯","D"
  ];
  var x = document.getElementById("rootSelect").selectedIndex;
  let y = document.getElementById("chordSelect").selectedIndex;
  let degLen = degNested[y].length;
  let scale = "<b>" + a + b + "</b>" + "<br>";
  for (let i = 0; i < degLen; i++) {
    scale += noteArray[rootNested[x][degNested[y][i]]-qualNested[y][i]] + " ";
  }
  const div = document.getElementById("scale");
  div.insertAdjacentHTML('beforeend', scale + "<br>");
  }
</script>
</body>
</html>
