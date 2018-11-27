# 分数

- ARIMA __0.1264__附近，与`andom_state`参数影响较大，最好可到**0.1260**；
- LGB __0.1305__，基于ARIMA模型，将ARIMA结果当做真实值构造相应特征；
- ARIMA+LGB __0.1270__，将两个模型结果简单的取平均，效果不如ARIMA



# 小技巧

- 根据评价函数`e=(np.log(1+y_true)-np.log(1+y_pred))^2`可以尝试将原始数据做`y=np.log(1+x)`变换，预测完成后再还原，有几万到1千左右的提升

- 根据观察可知`每天所有县城的流入=所有县城的流出`,故可对预测后的结果通过变换使得两者相等，当原始预测两者相差越大，该方法提升越明显。ARIMA方法可提升几万到1千。

- 利用评分规则取巧(有点类似于信息增益)

  >
  >
  >1.提交全部为0的结果，得到分数记为F1,可理解为原始信息熵
  >
  >2.只提交流入，流出和停留均为0，得到分数F2
  >
  >3.由于分数是效果越好越低，所以提交的流入的贡献是F1-F2，可理解为信息增益
  >
  >4.同理可计算流出的贡献和停留的贡献
  >
  >5.流入和流出贡献比可以作为技巧2的权重。

# 展望

- 个人觉得这道题重点在日期的处理上，特别是假期，因为只有假期特征才可以解释异常点，时间序列方法只能解释趋势项和周期项，所以对日期的反脱敏是这道题的突破口
- 也算是日期反脱敏的一部分，假如需要预测的20180302-20180316这段时间真实情况不属于节假日，那么有种比较好的思路
  - 将历史数据中存在异常点的数据删掉，注意要7天一起删，保证其周期性
  - 将异常点替换，比如3.15是异常值，当天是周日，那么用该县除异常值外的周日的平均值替换

- transition文件的使用，暂时还有使用到该文件，有一些初步想法
  - 和flow的使用一样，通过时间序列补全数据，然后统计县1->县2近1天统计情况，近7天统计情况等
  - 统计所有县两两之间的状态转移情况，构造马尔科夫过程，通过前一天状态预测下一天状态，状态过多可以使用分箱减少状态数
  - ...

- 最后吐槽一句:好垃圾的赛制，好垃圾的题目