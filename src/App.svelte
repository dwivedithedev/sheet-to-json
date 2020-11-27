<script>
	import { onMount } from 'svelte';
	import excelToJson from 'convert-excel-to-json';
	import prettyPrintJson from 'pretty-print-json';

	import Info from './Info.svelte';
	
	let flag="Awaiting file upload..";
	let data="Output will be here";

	let btn,modal;
	  
	const onChange = (e) => {
    const reader = new FileReader();
    reader.onloadend = function (event) {
      //console.log(event.target.result);
      const tmp = excelToJson({ source: event.target.result});
	  console.log(tmp); //this spits the output
	  data=tmp;

	  //show the pretty printed json
	  const elem = document.getElementById('account');
	  elem.innerHTML = prettyPrintJson.toHtml(data);

	  //function which exports it to a file to download
    };
	reader.readAsArrayBuffer(e.target.files[0]);
	flag="Success";
	document.querySelector('button').disabled = false;
  };

  	let saveFile = () => {
	const filename = 'data.json';
	const jsonStr = JSON.stringify(data);

	let element = document.createElement('a');
	element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(jsonStr));
	element.setAttribute('download', filename);

	element.style.display = 'none';
	document.body.appendChild(element);

	element.click();

	document.body.removeChild(element);
  }

  	let toggleAdvanced = () => {
		modal.style.display = "block";
	  }
		
	let closeModal = () =>{
		modal.style.display = "none";
	}

	  onMount(()=>{
		// Get the button that opens the modal
		btn = document.getElementById("myBtn");
		modal = document.getElementById("myModal");
	  })
</script>

<main>
	<h1>Sheet to JSON</h1>
	<p>Upload your <b>.xls</b> or <b>.xlsx</b> file and get it converted into JSON. That's it. That's all it does.</p>
	<input type="file" accept=".xls,.xlsx, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel" on:change={onChange} />
	<button on:click={saveFile} disabled>Export Json</button>
	
	<h3 class="textChoco">Results:- {flag}</h3>
	<pre id='account'></pre>


	<button class="infoBtn pulseAnim" on:click={toggleAdvanced}>👀</button>
	<!-- The Modal -->
	<div id="myModal" class="modal">
		<!-- Modal content -->
		<div class="modal-content">
			<div class="modal-footer">
				<span class="close" on:click={closeModal}>&times;</span>
				<Info />
			</div>
		</div>
	
	</div>
</main>

<style>
	::-moz-selection { /* Code for Firefox */
		color: #A39165;
		background: #64443A;
	}

		::selection {
		color: #A39165;
		background: #64443A;
	}

	.textChoco{
		color: #A39165;
	}
	pre {
		text-align: justify;
		font-size: medium;
		font-weight: 200;
		color: #64443A;	
	}
	main {
		text-align: center;
		padding: 1em;
		min-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #A39165;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 300;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.infoBtn {
    position: fixed;
    bottom: 0px;
    right: 0px; 
	margin: 22px;
	padding: 20px;
	border-radius: 50%;
	background-color: #A39165;
	}

	.pulseAnim {
    display: block;
    padding: 16px;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 0 rgba(88, 120, 243, 0.4);
    animation: pulseAnim 2s infinite;
}

@keyframes pulseAnim {
    0% {
        box-shadow: 0 0 0 0 rgba(100, 68, 58, 0.4);
    }
    70% {
        box-shadow: 0 0 0 10px rgba(100, 68, 58, 0);
    }
    100% {
        box-shadow: 0 0 0 0 rgba(100, 68, 58, 0);
    }
}

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
  -webkit-animation-name: fadeIn; /* Fade in the background */
  -webkit-animation-duration: 0.4s;
  animation-name: fadeIn;
  animation-duration: 0.4s
}

/* Modal Content */
.modal-content {
  position: fixed;
  bottom: 0;
  background-color: #fefefe;
  width: 100%;
  -webkit-animation-name: slideIn;
  -webkit-animation-duration: 0.4s;
  animation-name: slideIn;
  animation-duration: 0.4s
}

/* The Close Button */
.close {
  color: white;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}


.modal-footer {
  padding: 2px 16px;
  background-color: #64443A;
  color: white;
}

/* Add Animation */
@-webkit-keyframes slideIn {
  from {bottom: -300px; opacity: 0} 
  to {bottom: 0; opacity: 1}
}

@keyframes slideIn {
  from {bottom: -300px; opacity: 0}
  to {bottom: 0; opacity: 1}
}

@-webkit-keyframes fadeIn {
  from {opacity: 0} 
  to {opacity: 1}
}

@keyframes fadeIn {
  from {opacity: 0} 
  to {opacity: 1}
}
</style>
