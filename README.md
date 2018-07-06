# hivesql_from_qunar_arpu-
these hive sql are some project from qunar.（2018-7-1）

'''
【需求背景】计算拼拆红包项目ROI使用 
【需求描述】 
1、表头：6个月返前，12个月返前，6个月返后，12个月返后 
2、ARPU计算：全业务 
3、计算使用uid：见附件 
用户筛选规则：5月17日~6月10日，礼包领取（已剔除领取当日发起一级分享用户）后APP新下单用户，下单phone对应为uid
'''
arpu解释：（来源qunar的官方wiki）
简短版：
一.Arpu 的定义与用途

（ 1） Arpu 定义： 每个用户带来的平均收入

（ 2） 用户定义：

           如何定义用户：客户端设备口径（UID）

           如何界定用户生命周期：设备口径以激活后年为生命周期

（ 3） Arpu 用途：

           核算roi。除评估roi时需要用到arpu外，其他评估最好不要用arpu作为评估指标，尤其是活动效果评估，建议使用用户留存、复购等真实指标做活动评估。

           目前的应用场景仅有以下两大类：

           计算新激活用户Arpu：以激活设备（uid）为基础，计算每个激活用户（uid）从激活开始在生命周期中带来的预期平均收益（如激活时间超过12个月， 计算的是该设备号的真实收益）

           计算新交易用户Arpu：以新交易用户为基础，计算每个新交易用户(uid)从激活开始在生命周期中（12个月）带来的预期平均收益（如激活时间超过12个月， 计算的是该设备号的真实收益）。其中新交易用户的定义有以下几种方式为：客户端设备口径（UID）新用户；客户端手机号口径新用户；无线端手机号口径新用户(目前携程要求的口径，现在基本都用此口径)；无线端手机号+客户端设备号口径新用户。在此基础上，在新交易用户Arpu值计算维度上分去首单arpu值和正常arpu值。去首单arpu值是指去掉新交易用户的第一张订单后再计算arpu值。正常arpu值就是把新交易用户的全量订单都考虑进来。

（4）业务申请arpu值请发邮件到Arpu@qunar.com，邮件需写清楚要计算哪种arpu值，需提供的数据如下：

          需求方根据自己的情况选则应用场景，提供该应用场景的需要的数据，并告知按照新激活用户还是新交易用户(需确定是会否去首单)计算：

          新激活用户Arpu：活动带来的新激活设备号uid

          新交易用户正常Arpu：活动带来的符合场景的新交易用户定义下的新交易设备号uid

          新交易用户去首单Arpu：活动带来的符合场景的新交易用户定义下的新交易设备号uid，首单业务类型，首单时间
          
详细版：          
（1）Arpu定义：每个用户带来的平均收入
（2）用户定义：
如何定义用户：客户端设备口径（UID）

如何界定用户生命周期：设备口径以激活后年为生命周期

（3）Arpu用途：
目前的应用场景仅有以下两种：

计算新激活用户Arpu：以激活设备（uid）为基础，计算每个激活用户（uid）从激活开始在生命周期中带来的预期平均收益（如激活时间超过12个月， 计算的是该设备号的真实收益）

计算新交易用户Arpu：以新交易用户为基础，计算每个新交易用户(uid)从激活开始在生命周期中（12个月）带来的预期平均收益（如激活时间超过12个月， 计算的是该设备号的真实收益）。其中新交易用户的定义有以下几种方式为：客户端设备口径（UID）新用户；客户端手机号口径新用户；无线端手机号口径新用户(目前携程要求的口径，现在基本都用此口径)；无线端手机号+客户端设备号口径新用户

 在此基础上，在新交易用户Arpu值计算维度上分去首单arpu值和正常arpu值。去首单arpu值是指去掉新交易用户的第一张订单后再计算arpu值。正常arpu值就是把新交易用户的全量订单都考虑进来。

注：除评估roi时需要用到arpu外，最它评估最好不要用arpu作为评估指标，尤其是活动效果评估，建议使用用户留存、复购等真实指标做活动评估。
二.Arpu的计算原理
(1)Arpu基本计算原理
返现前：∑各业务单票收入*新客预期交易总票数/新客总数

返现后：∑各业务单票利润*新客预期交易总票数/新客总数

其中，单票利润 = 单票收入 - 各种补贴（红包、返现、券等所有补贴）的总成本

其中单票收入及单票利润均由财务给出

目前采用的方式是：

1、统计历史上的激活用户一年的总购票量y，依次统计这批用户激活后15天内购票量x1，30天内购票量x2，45天内购票量x3，以此类推，直到360天内购票量x24，分别用x／ｙ，得到每个时间段内购票量和总购票量的比例系数zi=xi/y

2、预测交易总票数：首先得到用户到目前为止真实购票数ticket，然后计算用户激活时间到今天的天数落在1中的哪个区间上，取区间对应的系数zi，用tikcet/zi得到预测交易总票数。假设用户激活到今天一共40天，那就取１中的z3，用ticket/z3得到预测票数

关于返现前、返现后arpu

目前有返现前arpu和返现后arpu两个概念，其中返现前arpu是指单票利润使用财务给出的返现前的利润，返现后arpu是指单票利润使用财务给出的返现后的利润

(2)某业务拉来App新客的Arpu值计算原理（以机票业务为例）
 

机票业务拉新客总Arpu计算 = 机票Arpu + 其他业务上交叉销售带来各业务的Arpu

 

机票拉新客定义：首次交易产品是机票的新交易用户

注意：一旦用户在某业务进行了首次交易，就被认定为某业务的新客，生命周期中可能产生的所有价值都将被记在该业务上（计入该用户总Arpu中），即：其后在其他业务交易产生的价值都不能再归属于别的业务上

三.ROI的原理、用途与计算方法
ROI的标准定义为投资回报率，即一笔投资在整个项目生命周期中产生的净收益（不含任何成本）对总投资额的占比，但目前为了能在Qunar中衡量渠道（或某一市场活动）的投入产出效率，简化为新的计算方法：

ROI = 总收益 / 总投入
1）当ROI>1时，则收益高于投入，投入效率较好，可以做

2）当ROI<1时，则收益不及投入，投入效率不佳，建议不做

3）ROI与1的关系是一个项目做与不做的标杆，也是ROI存在的意义

4）roi应该>0；1是缺省标杆，公司可以根据业务发展的具体情况设定一个高于1或者低于1的ROI值作为执行标杆

以渠道上计算ROI为例，某一渠道ROI即为：某渠道返现前arpu / 某渠道CPA（单激活成本）
以某市场活动的ROI计算为例即为：拉来新客的返现前arpu / 市场活动成本
其中，市场活动成本根据口径松紧不同，可以包含补贴成本（新客首单、老客补贴）、地推成本、渠道TAC成本等等，根据要求和口径不同来定包含哪些成本

注意：以下计算逻辑均不正确，因为分子为总收入（或除补贴外收入），分母为净亏损，相当于用收入与亏损做比，分子分母口径不对应，违背了ROI的本质和存在意义（即对比投入与产出的直接关系），且在活动不亏损时Arpu值为负数，在活动盈亏平衡时ROI为不可计算的极大值

错误：ROI = 拉新客返现前arpu / 活动净亏损（活动成本-arpu）

错误：ROI = 拉新客返现前去首单arpu / 活动净亏损（活动成本-arpu）

四.Arpu计算申请流程及需要准备的数据
1、首先需求方一定要明确数据组目前提供的arpu的用途及应用场景，详见第一部分，arpu计算方式详见第二部分Arpu的计算原理（1）
2、明确了arpu应用场景后，需求方需判断自己的需求是否符合arpu的应用场景，进而决定是否计算arpu。如认为可以计算，发邮件联系数据组计算arpu，收到邮件后，数据组会认为需求方已明确arpu用途。
        邮件收件人为：Arpu@qunar.com

3、根据需求方认为可以计算的应用场景，需要该应用场景的数据，并告知按照新激活用户还是新交易用户(需确定是会否去首单)计算：
(1)　计算激活用户arpu
激活用户arpu会返回需求方提供绝大部分uid

提供的数据：设备号uid

(2)新交易用户arpu
新交易用户arpu会返回需求方提供的uid中符合新交易用户定义的uid

提供的数据：

设备号uid，首单业务类型，首单时间

注意：由于需求方提供的uid存在不规范等问题，会有某些uid存在arpu极高的情况（arpu极高主要是该uid的订单特别多，并不是算法的问题），需求方需设定条件排除部分uid，不用这些uid的值计算平均价值


一年的arpu计算：

#!/usr/bin/env bash
TODAY=`date +%Y-%m-%d`

hive_base_settings="
  set mapred.child.java.opts=-Xmx900m;
  set mapreduce.reduce.memory.mb=8192;
  set mapreduce.reduce.java.opts='-Xmx3600M';
  set mapreduce.map.memory.mb=10000;
  set mapreduce.map.java.opts='-Xmx800M';
  set mapred.child.map.java.opts='-Xmx800M';
  set mapred.job.queue.name=wirelessdev;
  set mapreduce.job.name=dw_hotel_arpu_zhangmm.zhang;
  set mapred.job.priority=HIGHT;
  set hive.exec.compress.output=true;
  set hive.optimize.reducededuplication=false;
  set mapred.output.compression.codec=org.apache.hadoop.io.compress.GzipCodec;
  set hive.auto.convert.join.noconditionaltask=false;
  use wirelessdata;
"

local_path=$(cd $(dirname $0);pwd)
local_file="$local_path/arpu-year.txt"

process_date=`date -d "$1 last days" +%Y-%m-%d`
process_month1=`date -d "$process_date 1 month ago" +%Y-%m`

# arpu profit

declare -A PROFIT_AFTER_MAP  # 定义数组
declare -A PROFIT_BEFORE_MAP  # 定义数组

# 定义function得到“各业务的票收入”，从已知hive表（rdb_ss_bizincome）中获取
function get_arpu_profit() {
    local HIVE_BIZINCOME_CONF="sudo -uwirelessdev hive"
    local sql="use wirelessdata;select biz_type, income_pre from rdb_ss_bizincome_conf where month = '$1' and type = '$2';"
    local result="$(${HIVE_BIZINCOME_CONF} <<< ${sql})" # 先执行$()中的命令，然后将结果赋予前边的变量
    # shell脚本中不加local则默认为全局变量
    # 一次性将文件信息读入并赋值给变量line，while中使用重定向机制，文件中的所有信息都被读入并重定向给了整个while语句中的line变量
    while read line
    do
        read biz profit <<< $(awk '{print $1, $2}' <<< "${line}") # awk 对小数取整操作 # $()先完成括号中的命令行，然后将其结果替换出来，再重组成新的命令行
#        echo $biz,$profit
        if [ $2 = 'after' ]; then
            PROFIT_AFTER_MAP[$biz]="${profit}"
        elif [ $2 = 'bef' ]; then
            PROFIT_BEFORE_MAP[$biz]="${profit}"
        fi
    # 此结构体语句，首先将<<<后的内容一行行的赋值给line,然后循环体内再对line进行操作
    done <<< "$(echo -e "${result}")" 
}

# 获得各业务（例如：酒店、机票、旅游等）对应的收入
get_arpu_profit ${process_month1} 'after'

FLIGHT_AFTER=${PROFIT_AFTER_MAP[flight]}
HOTEL_AFTER=${PROFIT_AFTER_MAP[hotel]}
TRAIN_AFTER=${PROFIT_AFTER_MAP[train]}
TUAN_AFTER=${PROFIT_AFTER_MAP[tuan]}
VACATION_AFTER=${PROFIT_AFTER_MAP[vacation]}
PIAO_AFTER=${PROFIT_AFTER_MAP[piao]}
CAR_AFTER=${PROFIT_AFTER_MAP[car-zhuanche]}
ICAR_AFTER=${PROFIT_AFTER_MAP[icar]}
COACH_AFTER=${PROFIT_AFTER_MAP[coach]}
MOVIE_AFTER=${PROFIT_AFTER_MAP[movie]}
echo ${FLIGHT_AFTER} ${HOTEL_AFTER}

get_arpu_profit ${process_month1} 'bef'
FLIGHT_BEFORE=${PROFIT_BEFORE_MAP[flight]}
HOTEL_BEFORE=${PROFIT_BEFORE_MAP[hotel]}
TRAIN_BEFORE=${PROFIT_BEFORE_MAP[train]}
TUAN_BEFORE=${PROFIT_BEFORE_MAP[tuan]}
VACATION_BEFORE=${PROFIT_BEFORE_MAP[vacation]}
PIAO_BEFORE=${PROFIT_BEFORE_MAP[piao]}
CAR_BEFORE=${PROFIT_BEFORE_MAP[car-zhuanche]}
ICAR_BEFORE=${PROFIT_BEFORE_MAP[icar]}
COACH_BEFORE=${PROFIT_BEFORE_MAP[coach]}
MOVIE_BEFORE=${PROFIT_BEFORE_MAP[movie]}
echo ${FLIGHT_BEFORE} ${HOTEL_BEFORE}

# 1、首先计算获得每一个uid所对应的logdate（注册日期）
# 2、需要知道每个用户各业务的订单量（即uid所对应的用户的真实购票数）。从表ods_uid_phone_by_success_order中可以获得：uid,order_type（订单类型）,order_no（订单编号）,
# order_time（订单时间）,number（订单数量）。 同时还需要根据uid所对应的用户截止到目前的激活时间，来确定属于哪个区间。获得“time”字段（hive sql 中ceil为向上取整）
# 3、获得了表：uid_cnt time flight_num hotel_number ...... coach_number,每一行对应的每一个uid用户的各业务的订单数量。然后根据已经提供的表
# tmp_order_weight，表中有time可对应上表，此表为系数表，即zi=xi/y的结果表。拿上表比上tmp_order_weight表中的各个业务系数得到每个uid用户的预期票数
# 并得到表：type uid_cnt flight hotel tuan ......icar coach
# 4、根据arpu计算公式分别计算“返现前ARPU”“返现后ARPU”
# 5、半年的arpu需要结合系数表中12月的zi来计算

hql="
select g.uid,
    flight*${FLIGHT_BEFORE},flight*${FLIGHT_AFTER},
    hotel*${HOTEL_BEFORE},hotel*${HOTEL_AFTER},
    tuan*${TUAN_BEFORE},tuan*${TUAN_AFTER},
    train*${TRAIN_BEFORE},train*${TRAIN_AFTER},
    vacation*${VACATION_BEFORE},vacation*${VACATION_AFTER},
    piao*${PIAO_BEFORE},piao*${PIAO_AFTER},
    car*${CAR_BEFORE},car*${CAR_AFTER},
    icar*${ICAR_BEFORE},icar*${ICAR_AFTER},
    coach*${COACH_BEFORE},coach*${COACH_AFTER},
    flight*${FLIGHT_BEFORE}+hotel*${HOTEL_BEFORE}+tuan*${TUAN_BEFORE}+train*${TRAIN_BEFORE}+vacation*${VACATION_BEFORE}+piao*${PIAO_BEFORE}+car*${CAR_BEFORE}+icar*${ICAR_BEFORE}+coach*${COACH_BEFORE},
    flight*${FLIGHT_AFTER}+hotel*${HOTEL_AFTER}+tuan*${TUAN_AFTER}+train*${TRAIN_AFTER}+vacation*${VACATION_AFTER}+piao*${PIAO_AFTER}+car*${CAR_AFTER}+icar*${ICAR_AFTER}+coach*${COACH_AFTER}
from
    (
     select e.uid,f.time,
    flight_number/flight as flight,
    hotel_number/hotel as hotel,
    tuan_number/tuan as tuan,
    train_number/train as train,
    vacation_number/vacation as vacation,
    piao_number/piao as piao,
    car_number/car as car,
    icar_number/car as icar,
    coach_number/car as coach
 from
    (
    select d.uid,
        case when ceil(datediff('${TODAY}', logdate)/15)=0 then '1'
            when ceil(datediff('${TODAY}', logdate)/15)>=24 then '24'
            else ceil(datediff('${TODAY}', logdate)/15) end as time,                       
        sum(if(order_type='wap-flight',number,0)) as flight_number,
        sum(if(order_type='wap-hotel',number,0)) as hotel_number,
        sum(if(order_type='wap-tuan',number,0)) as tuan_number,
        sum(if(order_type='wap-train',number,0)) as train_number,
        sum(if(order_type='wap-vacation',number,0)) as vacation_number,
        sum(if(order_type='wap-piao',number,0)) as piao_number,
        sum(if(order_type='wap-car',number,0)) as car_number,
        sum(if(order_type='wap-icar',number,0)) as icar_number,
        sum(if(order_type='wap-coach',number,0)) as coach_number
     from
        (
         select distinct a.uid,logdate
         from
            (
             select distinct uid from uid_table
            ) a
            join
            (
             select logdate,lower(uid) as uid
             from ods_device_active
             where client='client' and platform in ('adr', 'ios') and pid in ('11010','10010','10030')
            ) b
                on a.uid=b.uid
        ) c
        join
        (
         select uid,order_type,order_no,order_time,number
         from ods_uid_phone_by_success_order
         where dict_type='all' and order_flag='1' and uid is not null and uid != ''
        ) d
            on c.uid=d.uid
            group by d.uid,
                case 
                    when ceil(datediff('${TODAY}', logdate)/15)=0 then '1'
                    when ceil(datediff('${TODAY}', logdate)/15)>=24 then '24'
                    else ceil(datediff('${TODAY}', logdate)/15) 
                end
        ) e
    left join
    (
     select time,flight,hotel,tuan,train,vacation,piao,car
     from tmp_order_weight
     where type='usual'
    ) f
    on e.time=f.time
    ) g;
"

# echo "${hql}"
sudo -u wirelessdev hive -e "${hive_base_settings};${hql}" > $local_file


if [ ! $? -eq 0 ] ; then
    echo 'hive查询失败！'
    exit 1
fi    

半年的arpu计算：

#!/usr/bin/env bash
TODAY=`date +%Y-%m-%d`

hive_base_settings="
  set mapred.child.java.opts=-Xmx900m;
  set mapreduce.reduce.memory.mb=8192;
  set mapreduce.reduce.java.opts='-Xmx3600M';
  set mapreduce.map.memory.mb=10000;
  set mapreduce.map.java.opts='-Xmx800M';
  set mapred.child.map.java.opts='-Xmx800M';
  set mapred.job.queue.name=wirelessdev;
  set mapreduce.job.name=dw_hotel_arpu_zhangmm.zhang;
  set mapred.job.priority=HIGHT;
  set hive.exec.compress.output=true;
  set hive.optimize.reducededuplication=false;
  set mapred.output.compression.codec=org.apache.hadoop.io.compress.GzipCodec;
  set hive.auto.convert.join.noconditionaltask=false;
  use wirelessdata;
"

local_path=$(cd $(dirname $0);pwd)
local_file="$local_path/arpu-year.txt"

process_date=`date -d "$1 last days" +%Y-%m-%d`
process_month1=`date -d "$process_date 1 month ago" +%Y-%m`

# arpu profit

declare -A PROFIT_AFTER_MAP  # 定义数组
declare -A PROFIT_BEFORE_MAP  # 定义数组

# 定义function得到“各业务的票收入”，从已知hive表（rdb_ss_bizincome）中获取
function get_arpu_profit() {
    local HIVE_BIZINCOME_CONF="sudo -uwirelessdev hive"
    local sql="use wirelessdata;select biz_type, income_pre from rdb_ss_bizincome_conf where month = '$1' and type = '$2';"
    local result="$(${HIVE_BIZINCOME_CONF} <<< ${sql})" # 先执行$()中的命令，然后将结果赋予前边的变量
    # shell脚本中不加local则默认为全局变量
    # 一次性将文件信息读入并赋值给变量line，while中使用重定向机制，文件中的所有信息都被读入并重定向给了整个while语句中的line变量
    while read line
    do
        read biz profit <<< $(awk '{print $1, $2}' <<< "${line}") # awk 对小数取整操作 # $()先完成括号中的命令行，然后将其结果替换出来，再重组成新的命令行
#        echo $biz,$profit
        if [ $2 = 'after' ]; then
            PROFIT_AFTER_MAP[$biz]="${profit}"
        elif [ $2 = 'bef' ]; then
            PROFIT_BEFORE_MAP[$biz]="${profit}"
        fi
    # 此结构体语句，首先将<<<后的内容一行行的赋值给line,然后循环体内再对line进行操作
    done <<< "$(echo -e "${result}")" 
}

# 获得各业务（例如：酒店、机票、旅游等）对应的收入
get_arpu_profit ${process_month1} 'after'

FLIGHT_AFTER=${PROFIT_AFTER_MAP[flight]}
HOTEL_AFTER=${PROFIT_AFTER_MAP[hotel]}
TRAIN_AFTER=${PROFIT_AFTER_MAP[train]}
TUAN_AFTER=${PROFIT_AFTER_MAP[tuan]}
VACATION_AFTER=${PROFIT_AFTER_MAP[vacation]}
PIAO_AFTER=${PROFIT_AFTER_MAP[piao]}
CAR_AFTER=${PROFIT_AFTER_MAP[car-zhuanche]}
ICAR_AFTER=${PROFIT_AFTER_MAP[icar]}
COACH_AFTER=${PROFIT_AFTER_MAP[coach]}
MOVIE_AFTER=${PROFIT_AFTER_MAP[movie]}
echo ${FLIGHT_AFTER} ${HOTEL_AFTER}

get_arpu_profit ${process_month1} 'bef'
FLIGHT_BEFORE=${PROFIT_BEFORE_MAP[flight]}
HOTEL_BEFORE=${PROFIT_BEFORE_MAP[hotel]}
TRAIN_BEFORE=${PROFIT_BEFORE_MAP[train]}
TUAN_BEFORE=${PROFIT_BEFORE_MAP[tuan]}
VACATION_BEFORE=${PROFIT_BEFORE_MAP[vacation]}
PIAO_BEFORE=${PROFIT_BEFORE_MAP[piao]}
CAR_BEFORE=${PROFIT_BEFORE_MAP[car-zhuanche]}
ICAR_BEFORE=${PROFIT_BEFORE_MAP[icar]}
COACH_BEFORE=${PROFIT_BEFORE_MAP[coach]}
MOVIE_BEFORE=${PROFIT_BEFORE_MAP[movie]}
echo ${FLIGHT_BEFORE} ${HOTEL_BEFORE}

# 1、首先计算获得每一个uid所对应的logdate（注册日期）
# 2、需要知道每个用户各业务的订单量（即uid所对应的用户的真实购票数）。从表ods_uid_phone_by_success_order中可以获得：uid,order_type（订单类型）,order_no（订单编号）,
# order_time（订单时间）,number（订单数量）。 同时还需要根据uid所对应的用户截止到目前的激活时间，来确定属于哪个区间。获得“time”字段（hive sql 中ceil为向上取整）
# 3、获得了表：uid_cnt time flight_num hotel_number ...... coach_number,每一行对应的每一个uid用户的各业务的订单数量。然后根据已经提供的表
# tmp_order_weight，表中有time可对应上表，此表为系数表，即zi=xi/y的结果表。拿上表比上tmp_order_weight表中的各个业务系数得到每个uid用户的预期票数
# 并得到表：type uid_cnt flight hotel tuan ......icar coach
# 4、根据arpu计算公式分别计算“返现前ARPU”“返现后ARPU”
# 5、半年的arpu需要结合系数表中12月的zi来计算

hql="
select g.uid,
    flight*${FLIGHT_BEFORE}*0.62,flight*${FLIGHT_AFTER}*0.62,
    hotel*${HOTEL_BEFORE}*0.62,hotel*${HOTEL_AFTER}*0.62,
    tuan*${TUAN_BEFORE}*0.61,tuan*${TUAN_AFTER}*0.61,
    train*${TRAIN_BEFORE}*0.62,train*${TRAIN_AFTER}*0.62,
    vacation*${VACATION_BEFORE}*0.72,vacation*${VACATION_AFTER}*0.72,
    piao*${PIAO_BEFORE}*0.62,piao*${PIAO_AFTER}*0.62,
    car*${CAR_BEFORE}*0.62,car*${CAR_AFTER}*0.62,
    icar*${ICAR_BEFORE}*0.62,icar*${ICAR_AFTER}*0.62,
    coach*${COACH_BEFORE}*0.62,coach*${COACH_AFTER}*0.62,
    flight*${FLIGHT_BEFORE}*0.62+hotel*${HOTEL_BEFORE}*0.62+tuan*${TUAN_BEFORE}*0.61+train*${TRAIN_BEFORE}*0.62+vacation*${VACATION_BEFORE}*0.72+piao*${PIAO_BEFORE}*0.62+car*${CAR_BEFORE}*0.62+icar*${ICAR_BEFORE}*0.62+coach*${COACH_BEFORE}*0.62,
    flight*${FLIGHT_AFTER}*0.62+hotel*${HOTEL_AFTER}*0.62+tuan*${TUAN_AFTER}*0.61+train*${TRAIN_AFTER}*0.62+vacation*${VACATION_AFTER}+piao*${PIAO_AFTER}*0.62+car*${CAR_AFTER}*0.62+icar*${ICAR_AFTER}*0.62+coach*${COACH_AFTER}*0.62
from
    (
     select e.uid,f.time,
    flight_number/flight as flight,
    hotel_number/hotel as hotel,
    tuan_number/tuan as tuan,
    train_number/train as train,
    vacation_number/vacation as vacation,
    piao_number/piao as piao,
    car_number/car as car,
    icar_number/car as icar,
    coach_number/car as coach
 from
    (
    select d.uid,
        case when ceil(datediff('${TODAY}', logdate)/15)=0 then '1'
            when ceil(datediff('${TODAY}', logdate)/15)>=24 then '24'
            else ceil(datediff('${TODAY}', logdate)/15) end as time,                       
        sum(if(order_type='wap-flight',number,0)) as flight_number,
        sum(if(order_type='wap-hotel',number,0)) as hotel_number,
        sum(if(order_type='wap-tuan',number,0)) as tuan_number,
        sum(if(order_type='wap-train',number,0)) as train_number,
        sum(if(order_type='wap-vacation',number,0)) as vacation_number,
        sum(if(order_type='wap-piao',number,0)) as piao_number,
        sum(if(order_type='wap-car',number,0)) as car_number,
        sum(if(order_type='wap-icar',number,0)) as icar_number,
        sum(if(order_type='wap-coach',number,0)) as coach_number
     from
        (
         select distinct a.uid,logdate
         from
            (
             select distinct uid from uid_table
            ) a
            join
            (
             select logdate,lower(uid) as uid
             from ods_device_active
             where client='client' and platform in ('adr', 'ios') and pid in ('11010','10010','10030')
            ) b
                on a.uid=b.uid
        ) c
        join
        (
         select uid,order_type,order_no,order_time,number
         from ods_uid_phone_by_success_order
         where dict_type='all' and order_flag='1' and uid is not null and uid != ''
        ) d
            on c.uid=d.uid
            group by d.uid,
                case 
                    when ceil(datediff('${TODAY}', logdate)/15)=0 then '1'
                    when ceil(datediff('${TODAY}', logdate)/15)>=24 then '24'
                    else ceil(datediff('${TODAY}', logdate)/15) 
                end
        ) e
    left join
    (
     select time,flight,hotel,tuan,train,vacation,piao,car
     from tmp_order_weight
     where type='usual'
    ) f
    on e.time=f.time
    ) g;
"

# echo "${hql}"
sudo -u wirelessdev hive -e "${hive_base_settings};${hql}" > $local_file


if [ ! $? -eq 0 ] ; then
    echo 'hive查询失败！'
    exit 1
fi    

