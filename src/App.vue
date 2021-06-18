<template>
  <div>
    <div>
      dhms
      <p>{{dhms}}</p>
    </div>
    <div>
      hms
      <p>{{hms}}</p>
    </div>
    <div>
      ms
      <p>{{ms}}</p>
    </div>
    <div>
      <p>s</p>
      <p>{{s}}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import HelloWorld from './components/HelloWorld.vue';
   /**
 * @preciseOption 'dhms' 'hms' 'ms' 's' 'd'
 * @endTime 为number则传入时间戳  为string的时候传入格式  YYYY-MM-DD HH:MM:SS 2021-06-18 18:00:00
 * @duration  2days 2hours 2minutes 2seconds
 * @localTimeStamp 当前的时间戳,必传
 * 不传obj默认返回 hms
 */
import dayjs from 'dayjs'
import duration from 'dayjs/plugin/duration';
dayjs.extend(duration);
export default defineComponent({
  name: 'App',
  components: {
    HelloWorld
  },
  setup()
  {
 
interface DOptions
{
    preciseOption:string
    duration:number,
    localTimeStamp:number
}
interface CountDownObj
{
    countDown:number
    countDownText:object
}
interface UnionType
{
    endTime?:test
    obj?:DOptions
}
type test=number | string
function isEndTime(parm:test):parm is test
{
    return typeof parm == 'string' || typeof parm == 'number'   
}
function formatCountDown(parm:UnionType)
{
    let countDown:number;
    let countDownText:object;
    let timeObj: duration.Duration
    let days:number;
    let hours:number;
    let minutes:number;
    let seconds:number;
    if(isEndTime(<test>parm.endTime))
    {
        let timeStamp=new Date(<test>parm.endTime).getTime();
        countDown=timeStamp-new Date().getTime();
        timeObj = dayjs.duration((countDown) / 1000, 'seconds');
        hours = Math.floor(timeObj.asHours());
        minutes = timeObj.minutes();
        seconds = timeObj.seconds()
        countDownText ={
            hours: hours,
            minutes: minutes,
            seconds: seconds,
        };
        return {countDown,countDownText}
    }
    else
    {
        const {preciseOption,duration,localTimeStamp}=<DOptions>parm.obj;
        
        switch(preciseOption)
        {
            case 'd':
            {
                //return dhms
                countDown=new Date(dayjs.duration({ days: duration }).asMilliseconds()+localTimeStamp-new Date().getTime()).getTime();
                timeObj = dayjs.duration(countDown / 1000, 'seconds');
                days=Math.floor(timeObj.asDays());
                hours = Math.floor(timeObj.asHours());
                minutes = timeObj.minutes();
                seconds = timeObj.seconds()
                countDownText ={
                    days:days,
                    hours: hours,
                    minutes: minutes,
                    seconds: seconds,
                };
                return {countDown,countDownText}
            }
            case 'h':
            {
                //return hms
                countDown=new Date(dayjs.duration({ hours: duration }).asMilliseconds()+localTimeStamp-new Date().getTime()).getTime();
                timeObj = dayjs.duration(countDown / 1000, 'seconds');
                hours = Math.floor(timeObj.asHours());
                minutes = timeObj.minutes();
                seconds = timeObj.seconds()
                countDownText ={
                    hours: hours,
                    minutes: minutes,
                    seconds: seconds,
                };
                return {countDown,countDownText}
            }
            case 'm':
            {
                //return ms
                countDown=new Date(dayjs.duration({ minutes: duration }).asMilliseconds()+localTimeStamp-new Date().getTime()).getTime();
                timeObj = dayjs.duration(countDown / 1000, 'seconds');
                minutes = timeObj.minutes();
                seconds = timeObj.seconds()
                countDownText ={
                    minutes: minutes,
                    seconds: seconds,
                };
                return {countDown,countDownText}
            }
            case 's':
            {
                //return s
                countDown=new Date(dayjs.duration({ seconds: duration }).asMilliseconds()+localTimeStamp-new Date().getTime()).getTime();
                timeObj = dayjs.duration(countDown / 1000, 'seconds');
                seconds = timeObj.seconds()
                countDownText ={
                    seconds: seconds,
                };
                return {countDown,countDownText}
            }
            default:
                return {countDown:0,countDownText:{}};
                
        }
    }
    
}
let timer:any;
const unChangedTimeStammp=new Date().getTime();

function calcCountDown(parm:UnionType,text:any) {
    let { countDown ,countDownText} =formatCountDown(parm);
    text.value=JSON.stringify(countDownText);
    timer = setTimeout(() => {
        // console.log(countDownText);
        
        countDown -= 1000;
        calcCountDown(parm,text);
        if(countDown<=0)
            clearTimeout(timer);
        
    }, 1000);
}
let dhms=ref('')
let hms=ref('')
let ms=ref('')
let s=ref('')
calcCountDown({obj:{preciseOption:"d",duration:1,localTimeStamp:unChangedTimeStammp}},dhms)
calcCountDown({obj:{preciseOption:"h",duration:1,localTimeStamp:unChangedTimeStammp}},hms)
calcCountDown({obj:{preciseOption:"m",duration:1,localTimeStamp:unChangedTimeStammp}},ms)
calcCountDown({obj:{preciseOption:"s",duration:9,localTimeStamp:unChangedTimeStammp}},s)
return {
  dhms,
  hms,
  ms,
  s
}
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
