<template>
    <div class="fixed z-10 top-0 w-full h-full flex bg-black bg-opacity-60">
        <div class="extraOutline p-4 bg-white w-max bg-whtie m-auto rounded-lg">
            <div class="file_upload p-5 relative border-4 border-dotted border-gray-300 rounded-lg" style="width:450px;">
                <svg class="text-indigo-500 w-24 mx-auto mb-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" /></svg>
                <div class="input_field flex flex-col w-max mx-auto text-center">
                    <div v-if="progress" class="progess-bar"><a @click="goToFile()"  style="color:blue; cursor: pointer;">https://files.yourcraft.pl/file/{{selectedFile.name}}</a></div>
                    <p class="text-white">.</p>
                    <label>
                        <input type="file" @change="onFileChange" class="text-sm cursor-pointer"/>

                    </label>
                    <p class="text-white">.</p>
                    <button @click="onUploadFile" class="upload-button text bg-indigo-600 text-white border border-gray-300 rounded font-semibold cursor-pointer p-1 px-3 hover:bg-indigo-500" :disabled="!this.selectedFile">Upload file</button>
                </div>      
            </div>      
        </div>
    </div>
    
</template>

<script>
import axios from "axios";
import { Notyf } from 'notyf';
import 'notyf/notyf.min.css';

export default {
    data() {
        return {selectedFile: "",progress: 0};
    },
    methods: {
    onFileChange(e) {
        const selectedFile = e.target.files[0];
        this.selectedFile = selectedFile;
        this.progress = 0;
        let name = this.selectedFile.name;
        console.log(name)
    },
    onUploadFile() {
        const formData = new FormData();
        formData.append("file", this.selectedFile); // appending file
        axios.post("http://localhost:3000/upload", formData, {
            onUploadProgress: ProgressEvent => {
            let progress = Math.round((ProgressEvent.loaded / ProgressEvent.total) * 100)+"%";
            this.progress = progress;
        }}).then(res => {
            const notyf = new Notyf({
                duration: 1000,
                position: {
                    x: 'right',
                    y: 'top',
                },
            }); 
            notyf.success('Plik zostal pomyslnie wyslany!');
          console.log(res);
        }).catch(err => {
            const notyf = new Notyf({
                duration: 1000,
                position: {
                    x: 'right',
                    y: 'top',
                },
                types: [{
                    type: 'error',
                    background: 'indianred',
                    duration: 5000,
                    dismissible: true
                }]
            }); 
            notyf.open({type: 'error',message: 'Wystapil blad podczas laczenia z serwerem!'});
            console.log(err);
        });
    },
    goToFile(){
        window.location.href = 'http://localhost:3000/'+this.selectedFile.name
    }
  }
};
</script>
<style scoped>
.upload-button:disabled {
  background-color: #b3bcc4;
  cursor: no-drop;
}
</style>