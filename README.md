<!-- 基本信息 -->
<h3 style="color: #007bff; border-bottom: 2px solid #007bff; padding-bottom: 5px;">1. 基本信息</h3>
<div class="input-group">
    <label>当前年龄（岁）：</label>
    <input type="number" id="currentAge" value="18" min="16" max="30">
</div>
<div class="input-group">
    <label>大学学制（年）：</label>
    <input type="number" id="collegeYears" value="4" min="2" max="6">
</div>
<div class="input-group">
    <label>折现率（如1%填0.01）：</label>
    <input type="number" id="discountRate" value="0.01" step="0.01" min="0.01" max="0.2">
</div>
<div class="input-group">
    <label>计划退休年龄（岁）（55-70）：</label>
    <input type="number" id="retirementAge" value="60" min="50" max="70">
</div>

<!-- 直接成本（大学期间每年固定支出） -->
<h3 style="color: #007bff; border-bottom: 2px solid #007bff; padding-bottom: 5px;">2. 大学直接成本（每年）</h3>
<div class="input-group">
    <label>学费（元）：</label>
    <input type="number" id="tuition" value="12000">
</div>
<div class="input-group">
    <label>住宿费（元）：</label>
    <input type="number" id="accommodation" value="1500">
</div>
<div class="input-group">
    <label>生活费（元）：</label>
    <input type="number" id="living" value="2000">
</div>
<div class="input-group">
    <label>其他学杂费（元）：</label>
    <input type="number" id="other" value="1000">
</div>

<!-- 大学期间收入 -->
<h3 style="color: #007bff; border-bottom: 2px solid #007bff; padding-bottom: 5px;">3. 大学期间收入</h3>
<div class="input-group">
    <label>大学每年奖学金（元）：</label>
    <input type="number" id="collegeScholarship" value="5000">
</div>
<div class="input-group">
    <label>大学每月兼职收入（元）：</label>
    <input type="number" id="collegeParttime" value="500">
</div>

<!-- 直接工作（高中毕业后的收入） -->
<h3 style="color: #007bff; border-bottom: 2px solid #007bff; padding-bottom: 5px;">4. 直接工作（高中毕业起点）</h3>
<div class="input-group">
    <label>起薪（元/月）：</label>
    <input type="number" id="directStartSalary" value="3000">
</div>
<div class="input-group">
    <label>年增长工资（元/年）：</label>
    <input type="number" id="directAnnualGrowth" value="100">
</div>
<div class="input-group">
    <label>工资封顶（元/月）：</label>
    <input type="number" id="directCap" value="7000">
</div>

<!-- 大学工作（毕业后的收入） -->
<h3 style="color: #007bff; border-bottom: 2px solid #007bff; padding-bottom: 5px;">5. 大学毕业工作</h3>
<div class="input-group">
    <label>起薪（元/月）：</label>
    <input type="number" id="collegeStartSalary" value="6500">
</div>
<div class="input-group">
    <label>年增长工资（元/年）：</label>
    <input type="number" id="collegeAnnualGrowth" value="300">
</div>
<div class="input-group">
    <label>工资封顶（元/月）：</label>
    <input type="number" id="collegeCap" value="30000">
</div>

<button onclick="calculate()">立即计算并决策</button>

<!-- 结果展示 -->
<div class="result" id="result">
    <h3>决策结果</h3>
    <p>直接工作总净现值（NPV）：<span class="highlight" id="directNpv"></span> 元</p>
    <p>读大学总净现值（NPV）：<span class="highlight" id="collegeNpv"></span> 元</p>
    <p id="recommendation"></p>
</div>
</div>

<script>
function calculate() {
    // JavaScript计算逻辑
    // 获取输入值、计算NPV、生成决策建议
}
</script>
