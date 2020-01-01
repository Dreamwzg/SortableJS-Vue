 <template>
   <div class="table-body">
     <el-table v-if="show" border :span-method="objectSpanMethod" row-key="rowIndex" :data="tableData" style="width: 100%">
       <el-table-column prop="position" label="position" width="180">
       </el-table-column>
       <el-table-column prop="date" label="日期" width="180">
       </el-table-column>
       <el-table-column prop="name" label="姓名" width="180">
       </el-table-column>
       <el-table-column prop="address" label="地址">
       </el-table-column>
     </el-table>
     <el-table v-if="two" border :span-method="objectSpanMethod" row-key="rowIndex" :data="tableData" style="width: 100%">
       <el-table-column prop="position" label="position" width="180">
       </el-table-column>
       <el-table-column prop="date" label="日期" width="180">
       </el-table-column>
       <el-table-column prop="name" label="姓名" width="180">
       </el-table-column>
       <el-table-column prop="address" label="地址">
       </el-table-column>
     </el-table>
   </div>
 </template>

 <script>
 import Sortable from 'sortablejs';
   export default {
     data() {
       return {
         two:false,
         show:true,
         spanArr:[],
         tableData: [{
           id: '1',
           position:'1',
           rowIndex:1,
           date: '2016-05-02',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1518 弄'
         }, {
           id: '1',
           position:'1',
           rowIndex:"2",
           date: '2016-05-04',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1517 弄'
         },{
           id: '1',
           position:'1',
           rowIndex:"3",
           date: '2016-05-04',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1517 弄'
         }, {
           id: '4',
           position:'2',
           rowIndex:"4",
           date: '2016-05-01',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1519 弄'
         }, {
           id: '5',
           position:'3',
           rowIndex:"5",
           date: '2016-05-03',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1516 弄'
         },{
           id: '6',
           position:'4',
           rowIndex:"6",
           date: '2016-05-03',
           name: '王小虎',
           address: '上海市普陀区金沙江路 1516 弄'
         }]
       }
     },
     methods: {
       objectSpanMethod({
         row,
         column,
         rowIndex,
         columnIndex
       }) {
         if (columnIndex === 0)  {
           if (this.spanArr[rowIndex]) {
             return {
               rowspan: this.spanArr[rowIndex].length,
               colspan: 1
             };
           } else {
             return {
               rowspan:0,
               colspan: 1
             };
           }
         }

       },
       calSpanArr() {
         const map = {};
         const arr = [];
         let curPos = 0;
         this.tableData.forEach((item,index)=>{
           if(index === 0) {
             arr[0]=[item];
           }else {
             if (item.position === this.tableData[index-1].position) {
               arr[curPos].push(item);
             }else {
               curPos = index;
               arr[index] = [item];
             }
           }
         })
         this.spanArr = arr;
       },
       initDrag() {
         const that = this;
         const tbody = document.querySelector('.el-table tbody');
         Sortable.create(tbody,{
           animation:150,
           onEnd({newIndex,oldIndex}){
             const mid = that.tableData[oldIndex]
             const restOldArr = [];
             const restNewArr = [];
             const oldPosition = that.tableData[oldIndex].position;
             const newPosition = that.tableData[newIndex].position;
             if(oldPosition === newPosition) return;
             that.tableData.forEach(item=>{
               if (item.position === oldPosition) {
                 item.position = newPosition
               }else {
                 if (item.position === newPosition) {
                   item.position = oldPosition
                 }
               }
             })
             const tempArr = [];
             that.tableData.forEach(item=>{
               if(tempArr[item.position*1]) {
                    tempArr[item.position*1].push(item);
               }else {
                 tempArr[item.position*1] = [item]
               }
             });
             that.tableData = tempArr.reduce((init,item)=>{
               init.push(...item);
               return init
             },[]);
             that.calSpanArr(); 
            //  that.two = true;
             that.show = false;
             that.$nextTick(()=>{
               that.show = true;
               that.$nextTick(()=>{
                    that.initDrag();
               })
             })
          
            //  that.$nextTick(()=>{
            //    that.initDrag();
            //  })
            //  debugger;
             //node
            //  that.tableData[oldIndex] = that.tableData[newIndex];
            //  that.tableData[newIndex] = mid;
            //  that.tableData[oldIndex].isDrag = true;
            //  that.tableData[newIndex].isDrag = true;
            //  that.tableData.forEach(item=>{
            //    if (!item.isDrag) { 
            //        if(item.position === oldPosition) {
            //          restOldArr.push(item);
            //        }
            //        if(item.position === newPosition) {
            //          restNewArr.push(item)
            //        }
            //    }
            //  });
            //  const result = [];
            //  that.tableData.forEach(item=>{
                
            //  })

             console.log(that.tableData);
              console.log('newIndex',newIndex)
              console.log('oldIndex',oldIndex)
           }
         })
       }
     },
     mounted() {
       this.calSpanArr();
       this.initDrag();
     }
   }

 </script>


 <!-- Add "scoped" attribute to limit CSS to this component only -->
 <style scoped>
   /* @import url('https://unpkg.com/element-ui/lib/theme-chalk/index.css'); */
   h1,
   h2 {
     font-weight: normal;
   }

   ul {
     list-style-type: none;
     padding: 0;
   }

   li {
     display: inline-block;
     margin: 0 10px;
   }

   a {
     color: #42b983;
   }

   .table-body {
     /* border: 1px solid red; */
     /* display: inline-block; */
     width: 800px;
     margin: 0 auto;
   }

 </style>
