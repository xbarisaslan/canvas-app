<template>
 <div id="app">
    <div class=output>
      <canvas id="c" width="1070" height="760">
    
      </canvas>
    </div>
    <form id="settings">
      <h1>Toolbox</h1>
      <input type="button" name="text" value="Add Text" id="text" @click="addTextbox()"/>   
      <input type="button" name="rectangle" value="Add Rectangle" @click="addRectangle()"/>
      <input type="button" name="triangle" value="Add Triangle" @click="addTriangle()"/>
      <input type="button" name="circle" value="Add Circle" @click="addCircle()"/>   
      <input type="file" name="image" id="image" accept="image/*" @change="addImage()"  hidden  />  
      <label for="image">Add Image</label>

      <div id="font-size"> Font Size:
      <input type="number" id="font-size-input" min="1" max="50" value="25" @change="changeFontSize(this)"/>
      </div>

      <div id="font-family">Font family: 
        <select id="font-family-input" @change="changeFontFamily(this)">
          <option value="arial">Arial</option>
          <option value="helvetica" selected>Helvetica</option>
          <option value="myriad pro">Myriad Pro</option>
          <option value="delicious">Delicious</option>
          <option value="verdana">Verdana</option>
          <option value="georgia">Georgia</option>
          <option value="courier">Courier</option>
          <option value="comic sans ms">Comic Sans MS</option>
          <option value="impact">Impact</option>
          <option value="monaco">Monaco</option>
          <option value="optima">Optima</option>
          <option value="hoefler text">Hoefler Text</option>
          <option value="plaster">Plaster</option>
          <option value="engagement">Engagement</option>
        </select>
      </div>

      <div id="text-color">Change Color:
        <input type="color" id="text-color-input" value="#9022FF" size="50" @change="changeColor(this)"/>
      </div>

      <div id="opacity">Change Opacity:
        <input type="range" id="opacity-input" min="0" max="1" step="0.1" @change="changeImageOpacity(this)"/>
      </div>
    </form> 
  </div>
</template>

<script>
const fabric = require("fabric").fabric;
let canvas = null
let reader = null

export default {
  name:'App',
  mounted() {
      canvas = new fabric.Canvas('c');
      reader = new FileReader()
  },

  methods: {
    addRectangle() {
      const rect = new fabric.Rect({
      fill:'green',
      width:150,
      height:100,
  })
      canvas.add(rect);
      canvas.renderAll()
  },

    addCircle() {
      const circle = new fabric.Circle({
      radius:50,
      fill:'orange'
  })
      canvas.add(circle);
      canvas.renderAll()
  },

    addTriangle() {
      const triangle = new fabric.Triangle({
      width:100,
      height:100,
      fill:'red',
  })
      canvas.add(triangle);
      canvas.renderAll()
  },

    addTextbox() {
      let textbox = new fabric.Textbox("Sample Text", {
      width:100,
      height:100,
      fontSize:25,
      fontFamily:'Arial',
      fill:'red'
  });
      canvas.add(textbox);
      canvas.setActiveObject(textbox);
      canvas.renderAll();
  },

    addImage() {
      const imgElement = document.getElementById('image');
      const file = imgElement.files[0];
      reader.readAsDataURL(file)
      reader.addEventListener('load', () => {
          fabric.Image.fromURL(reader.result , image => {
          image.set({
            opacity:1
          }) 
          image.scaleToWidth(200)
          canvas.add(image);
          canvas.setActiveObject(image)
          canvas.renderAll()
        })
      })

      
    },
    
    changeFontSize() {
      let inputSize = document.getElementById("font-size-input")
      let obj = canvas.getActiveObject();
      let value = inputSize.value
      obj.set("fontSize",value);
      console.log(obj.fontSize)
      console.log(value)
      canvas.renderAll()
    },

    changeFontFamily() {
      let input = document.getElementById("font-family-input")
      let obj = canvas.getActiveObject()
      let value = input.value
      obj.set("fontFamily",value)
      canvas.renderAll()
    },

    changeColor() {
      let input = document.getElementById("text-color-input")
      let obj = canvas.getActiveObject();
      let value = input.value;
      obj.set("fill",value);
      canvas.renderAll()
    },

    changeImageOpacity() {
      let input = document.getElementById("opacity-input")
      let obj = canvas.getActiveObject()
      let value = input.value;
      obj.set("opacity",value);
      canvas.renderAll()
      console.log(obj)
    }
}}
</script>

<style>
* {
  font-family: Arial, Helvetica, sans-serif;
  box-sizing: border-box;
  margin:0;
}

#app {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  height: 100vh;
  overflow-x: hidden;
}

.output {
  background-color: rgb(247, 234, 211);
  width:70vw;
}

#settings {
  width: 30vw;
  padding:1rem;
}

h1 {
  text-align: center;
  padding-top: .5rem;
  color:rgb(83, 81, 79)
}

input,label{
  width: fit-content;
  padding: .5rem;
  margin:1rem .4rem;
  border:3px solid #F2F2F2;
  border-radius: 5px;
  font-size: 1rem;
  background-color: white;
  cursor: pointer;
} 

input#image {
  width: fit-content;
  padding: .5rem .3rem;
}

#font-size {
  display: flex;
  justify-content: center;
  align-items: center;
  width:25.7rem;
  padding: .5rem;
  outline:none;
  border: 2px solid #F2F2F2;
  margin-left: .5rem;
  margin-bottom: 1.6rem;
  margin-top: 1rem;
}

#font-size-input {
  border:2px solid #efe9e9;
  margin:0;
  padding: .2rem;
  padding-right: 0;
  margin-left: .4rem;
}

#font-family {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: .8rem;
  width:25.7rem;
  outline:none;
  border: 2px solid #F2F2F2;
  margin-left: .5rem;
  margin-bottom: 1.6rem;
}

select {
  outline: none;
  border: 2px solid #efe9e9;
  margin: 0 .5rem;
}

input[type="color"] { 
  appearance: auto;
	-webkit-appearance: none;
	border: none;
	height: 50px;
  cursor:crosshair;
}

input[type="color"]::-webkit-color-swatch-wrapper {
  padding: 0;
}

input[type="color"]::-webkit-color-swatch {
  border: none;
  border-radius: 1000px;
}

#text-color {
  display: flex;
  justify-content: center;
  align-items: center;
  width:25.7rem;
  outline:none;
  border: 2px solid #F2F2F2;
  margin-left: .5rem;
  margin-bottom: 1.6rem;
  margin-top: 1rem;
}

#text-color-input {
  width:3rem;
  height:3rem;
  border:none;
  border-radius: 1000px;
  cursor:crosshair;
}

#opacity {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid #F2F2F2;
  margin: .5rem;
}
</style>