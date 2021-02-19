<template>
  <div class="container">
    <div class="table">
      <md-table>
        <md-table-row>
            <md-table-head>Title</md-table-head>
            <md-table-head>Company</md-table-head>
            <md-table-head>Ticker</md-table-head>
            <md-table-head>Price</md-table-head>
            <md-table-head>Volume</md-table-head>
        </md-table-row>
        <md-table-row v-for="(data) in tableData" :key="data.title">
          <md-table-cell>{{data.title}}</md-table-cell>
          <md-table-cell>{{data.company}}</md-table-cell>
          <md-table-cell>{{data.ticker}}</md-table-cell>
          <md-table-cell>{{data.last}}</md-table-cell>
          <md-table-cell>{{data.volume}}</md-table-cell>
        </md-table-row>
      </md-table>
      <h6 class="font-italic">Disclaimer: data is emulated and does not reflect the actual market data.</h6>
    </div>
  </div>
</template>

<script>
import { StreamDataIo } from 'streamdataio-js-sdk'
import * as jsonpatch from 'fast-json-patch'

export default {
  name: 'stockmarket',
  data () {
    return {
      streamData: null,
      tableData: []
    }
  },
  created: function () {
    this.streamData =
           StreamDataIo.createEventSource(
             'http://stockmarket.streamdata.io/v2/prices',
             'ZDMwMDQ1NGEtNjA4OC00NTdmLWFhYjgtNDI0OTg3NTM2ZTFm',
             [])

    this.streamData.onData(data => {
      // initialize your data with the initial snapshot
      console.log('Received data')
      this.tableData = data
      console.table(this.tableData)
    }, this).onPatch(patch => {
      // update the data with the provided patch
      console.log('received patch %o', patch)
      jsonpatch.applyPatch(this.tableData, patch)
      console.table(this.tableData)
    }, this).onError(error => {
      // do whatever you need in case of error
      console.log('error: %o', error)
    }, this).onOpen(() => {
      this.isConnected = true
      // you can also add custom behavior when the stream is opened
      console.log('open')
    }, this)

    this.streamData.open()
  },
  destroyed: function () {
    this.streamData.close()
  }

}
</script>

<style>
.md-table-cell, .md-table-head {
  width: 33%;
  text-align: center;
}

.md-table-head {
  background-color: lightsteelblue;
}
</style>
