<script setup>
import 地球科学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_地球科学.json'
import 物理与天体物理 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_物理与天体物理.json'
import 数学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_数学.json'
import 农林科学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_农林科学.json'
import 材料科学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_材料科学.json'
import 计算机科学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_计算机科学.json'
import 环境科学与生态学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_环境科学与生态学.json'
import 化学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_化学.json'
import 工程技术 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_工程技术.json'
import 生物学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_生物学.json'
import 医学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_医学.json'
import 综合性期刊 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_综合性期刊.json'
import 法学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_法学.json'
import 心理学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_心理学.json'
import 教育学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_教育学.json'
import 经济学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_经济学.json'
import 管理学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_管理学.json'
import 人文科学 from '../../../../../electron-app/src/renderer/src/assets/中科院2022升级版_人文科学.json'

const str2JSON = (str) => {
  str = str.replace(/'/g, '"')
  let parse = JSON.parse(str)
  let keys = Object.keys(parse)
  let values = []
  for (let key in keys) values.push(parse[keys[key]])
  return [keys, values]
}

const getTypeByRank = (rank) => {
  if (rank === '1') return 'danger'
  else if (rank === '2') return 'warning'
  else if (rank === '3') return 'success'
  else return ''
}
const filterHandler = (value, row, column) => {
  console.log(value)
  const property = column['property']
  return row[property] == value
}

import { ref } from 'vue'

const defaultSubject = '无'
let subject = ref(defaultSubject)
let journalName = ref('')
const SUBJECTS = [
  { subject: defaultSubject },
  { subject: '地球科学' },
  { subject: '物理与天体物理' },
  { subject: '数学' },
  { subject: '农林科学' },
  { subject: '材料科学' },
  { subject: '计算机科学' },
  { subject: '环境科学与生态学' },
  { subject: '化学' },
  { subject: '工程技术' },
  { subject: '生物学' },
  { subject: '医学' },
  { subject: '综合性期刊' },
  { subject: '法学' },
  { subject: '心理学' },
  { subject: '教育学' },
  { subject: '经济学' },
  { subject: '管理学' },
  { subject: '人文科学' }
]
const searchJournalByName = (data, name) => {
  if (name != '') {
    return data.filter((row) => {
      return Object.keys(row).some(() => {
        return String(row['刊名']).toLowerCase().indexOf(name) > -1
      })
    })
  }
  return data
}
const CheckSubject = (subject, journalName) => {
  let data = null
  switch (subject) {
    case '地球科学':
      data = 地球科学
      break
    case '物理与天体物理':
      data = 物理与天体物理
      break
    case '数学':
      data = 数学
      break
    case '农林科学':
      data = 农林科学
      break
    case '材料科学':
      data = 材料科学
      break
    case '计算机科学':
      data = 计算机科学
      break
    case '环境科学与生态学':
      data = 环境科学与生态学
      break
    case '化学':
      data = 化学
      break
    case '工程技术':
      data = 工程技术
      break
    case '生物学':
      data = 生物学
      break
    case '医学':
      data = 医学
      break
    case '综合性期刊':
      data = 综合性期刊
      break
    case '法学':
      data = 法学
      break
    case '心理学':
      data = 心理学
      break
    case '教育学':
      data = 教育学
      break
    case '经济学':
      data = 经济学
      break
    case '管理学':
      data = 管理学
      break
    case '人文科学':
      data = 人文科学
      break
    default:
      data = null
  }
  return searchJournalByName(data, journalName)
}
</script>

<template>
  <div>
    <div>
      <el-select v-model="subject" style="width: 100%" :placeholder="defaultSubject" size="large">
        <el-option
          v-for="s in SUBJECTS"
          :key="s.index"
          :value="s.subject"
          style="font-size: 20px"
        ></el-option>
      </el-select>
    </div>
    <div>
      <input
        v-model.lazy="journalName"
        placeholder="按下回车查询"
        type="text"
        style="width: 92%; height: 26px; margin-top: 10px; font-size: 20px"
      />
    </div>
  </div>
  <div>
    <el-table stripe :data="CheckSubject(subject, journalName)" style="width: 100%" height="350">
      <el-table-column sortable prop="刊名" label="期刊名" width="150"></el-table-column>
      <el-table-column v-slot="scope" width="500" label="相关学科" prop="学科信息">
        <div v-for="(val, index) in str2JSON(scope.row.学科信息)[0]" :key="index">
          <el-tag :type="getTypeByRank(str2JSON(scope.row.学科信息)[1][index])"
            >{{ val + '-' }}{{ str2JSON(scope.row.学科信息)[1][index] + '\n' }}</el-tag
          >
        </div>
      </el-table-column>
      <el-table-column
        prop="分区"
        label="分区"
        :filters="[
          { text: 'Q1', value: '1' },
          { text: 'Q2', value: '2' },
          { text: 'Q3', value: '3' },
          { text: 'Q4', value: '4' }
        ]"
        :filter-method="filterHandler"
      >
      </el-table-column>
      <el-table-column prop="最低档" label="最低档"></el-table-column>
      <el-table-column prop="综述" label="综述"></el-table-column>
      <el-table-column prop="开放" label="对外开放"></el-table-column>
    </el-table>
  </div>
</template>
<style scoped></style>
