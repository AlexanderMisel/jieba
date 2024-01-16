jieba-newdict
========
“结巴”中文分词是曾经最好的 Python 中文分词组件。本项目维护者不直接使用 jieba，而用的[自己魔改过的 deno-jieba](https://github.com/brynne8/deno-jieba)，这个魔改可以避免加载默认字典，只使用自定义字典。

我大概是这样使用的：

```js
import { reset, withDict, CutMode, tag } from "./mod.ts";
await withDict('./dict_revised.txt');

const flag_en2cn = {
  a: '形容词', ad: '副形词', ag: '形语素', an: '名形词', b: '区别词',
  c: '连词', d: '副词', df: '不要', dg: '副语素',
  e: '叹词', f: '方位词', g: '语素', h: '前接成分',
  i: '成语', j: '简称略语', k: '后接成分', l: '习用语',
  m: '数词', mg: '数语素', mq: '数量词',
  n: '名词', ng: '名语素', nr: '人名', nrfg: '古代人名', nrt: '音译人名',
  ns: '地名', nt: '机构团体', nz: '其他专名',
  o: '拟声词', p: '介词', q: '量词',
  r: '代词', rg: '代语素', rr: '代词', rz: '人称代词',
  s: '处所词', t: '时间词', tg: '时间语素',
  u: '助词', ud: '得', ug: '过', uj: '的', ul: '了', uv: '地', uz: '着',
  v: '动词', vd: '副动词', vg: '动语素', vi: '动词', vn: '名动词', vq: '动词',
  va: '动形词', vx: '趋向动词', vl: '连系动词', vm: '实义动词',
  x: '非语素字', y: '语气词', z: '状态词', zg: '状态语素',
}

// vu助动词
// va动形词是一种既能做动词也能做形容词的类型，如“不满”，有时甚至可以认为是名词。
// vx趋向动词是用来区分一般动词的一种表示要去什么地方的一种动词，在其他实义动词存在的情况下不会做谓语
// vm实义动词，后面不可再合并动词

const jbDict = {};
const logN = 7.778888685701;
let jbFile = await Deno.readTextFile('./dict_revised.txt')
jbFile.split('\n').map(x => {
  let [ word, freq, tag ] = x.split(' ');
  jbDict[word] = { freq: Math.log10(freq) - logN, tag: tag };
})
```

增加了几种动词类型，对于词性标注有些帮助。
