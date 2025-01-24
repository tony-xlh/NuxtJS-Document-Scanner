<template>
  <div>
    <h2>Document Scanner</h2>
    <DocumentScanner @onWebTWAINReady="onWebTWAINReady"></DocumentScanner>
    <button @click="scan">Scan</button>
    <button @click="save">Save as PDF</button>
  </div>
</template>
<script setup lang="ts">
import { WebTwain } from 'dwt/dist/types/WebTwain';
let DWTObject:WebTwain|undefined;
const onWebTWAINReady = (dwt:WebTwain) => {
  DWTObject = dwt;
};

const scan = ()=> {
  if (DWTObject) {
    DWTObject.SelectSourceAsync()
      .then(function () {
        return DWTObject.AcquireImageAsync({
          IfCloseSourceAfterAcquire: true,
        });
      })
      .catch(function (exp) {
        alert(exp.message);
      });
  }
}

const save = ()=> {
  if (DWTObject) {
    DWTObject.SaveAllAsPDF("Scanned.pdf");
  }
}
</script>
