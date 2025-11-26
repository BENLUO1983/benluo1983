<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>人力资本投资决策工具</title>
    <style>
        body { font-family: "微软雅黑", Arial; max-width: 850px; margin: 20px auto; padding: 0 20px; }
        .container { background: #f0f8ff; padding: 25px; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
        .input-group { margin: 18px 0; display: flex; align-items: center; }
        label { width: 280px; font-size: 14px; color: #333; }
        input { width: 180px; padding: 8px 12px; border: 1px solid #ddd; border-radius: 5px; font-size: 14px; }
        button { padding: 12px 30px; background: #28a745; color: white; border: none; border-radius: 5px; cursor: pointer; font-size: 16px; margin-top: 10px; }
        button:hover { background: #218838; }
        .result { margin-top: 25px; padding: 20px; background: #fff3cd; border-radius: 5px; border-left: 5px solid #ffc107; display: none; }
        .result h3 { margin-top: 0; color: #856404; }
        .result p { margin: 10px 0; line-height: 1.6; }
        .highlight { color: #dc3545; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <h2 style="text-align: center; color: #007bff;">人力资本投资决策工具</h2>
        
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
            // 1. 获取所有输入值（转为数字）
            const currentAge = +document.getElementById('currentAge').value;
            const collegeYears = +document.getElementById('collegeYears').value;
            const discountRate = +document.getElementById('discountRate').value;
            const retirementAge = +document.getElementById('retirementAge').value;
            
            // 直接成本
            const tuition = +document.getElementById('tuition').value;
            const accommodation = +document.getElementById('accommodation').value;
            const living = +document.getElementById('living').value;
            const other = +document.getElementById('other').value;
            
            // 直接工作（高中毕业）
            const directStartSalary = +document.getElementById('directStartSalary').value;
            const directAnnualGrowth = +document.getElementById('directAnnualGrowth').value;
            const directCap = +document.getElementById('directCap').value;
            
            // 大学工作（毕业）
            const collegeStartSalary = +document.getElementById('collegeStartSalary').value;
            const collegeAnnualGrowth = +document.getElementById('collegeAnnualGrowth').value;
            const collegeCap = +document.getElementById('collegeCap').value;
            
            // 大学期间收入
            const collegeScholarship = +document.getElementById('collegeScholarship').value;
            const collegeParttime = +document.getElementById('collegeParttime').value;


            // 2. 计算毕业年龄
            const graduationAge = currentAge + collegeYears;
            if (graduationAge > retirementAge) {
                alert("毕业年龄超过退休年龄，请调整参数！");
                return;
            }


            // 3. 计算直接工作的NPV（从当前年龄到退休）
            let directNpv = 0;
            for (let age = currentAge; age <= retirementAge; age++) {
                const yearsPassed = age - currentAge;
                // 当前年薪 =  min(起薪 + 年增长×工龄, 封顶工资) ×12
                const currentDirectSalary = Math.min(
                    directStartSalary + yearsPassed * directAnnualGrowth,
                    directCap
                ) * 12;
                // 折现到当前（现值=未来值/(1+折现率)^年数）
                directNpv += currentDirectSalary / Math.pow(1 + discountRate, yearsPassed);
            }


            // 4. 计算大学期间的净现金流现值（从当前到毕业）
            let collegeCashFlowNpv = 0;
            for (let age = currentAge; age < graduationAge; age++) {
                const yearsPassed = age - currentAge;
                
                // a. 大学期间每年固定成本（直接成本）
                const annualDirectCost = tuition + accommodation + living + other;
                
                // b. 机会成本：当年放弃的直接工作收入
                const currentDirectSalary = Math.min(
                    directStartSalary + yearsPassed * directAnnualGrowth,
                    directCap
                ) * 12;
                
                // c. 大学期间每年收入（奖学金+兼职）
                const collegeAnnualIncome = collegeScholarship + collegeParttime * 12;
                
                // d. 当年净现金流 = 大学收入 - （直接成本 + 机会成本）
                const annualNetCash = collegeAnnualIncome - (annualDirectCost + currentDirectSalary);
                
                // e. 折现到当前
                collegeCashFlowNpv += annualNetCash / Math.pow(1 + discountRate, yearsPassed);
            }


            // 5. 计算大学毕业后的收入现值（从毕业到退休）
            let collegePostGradNpv = 0;
            for (let age = graduationAge; age <= retirementAge; age++) {
                const yearsPassed = age - currentAge;
                const yearsSinceGrad = age - graduationAge;
                
                // 毕业后年薪 = min(大学起薪 + 年增长×毕业工龄, 封顶工资) ×12
                const currentCollegeSalary = Math.min(
                    collegeStartSalary + yearsSinceGrad * collegeAnnualGrowth,
                    collegeCap
                ) * 12;
                
                collegePostGradNpv += currentCollegeSalary / Math.pow(1 + discountRate, yearsPassed);
            }


            // 6. 读大学的总NPV = 大学期间净现金流现值 + 毕业后收入现值
            const collegeTotalNpv = collegeCashFlowNpv + collegePostGradNpv;


            // 7. 生成决策建议
            let recommendationHtml = "";
            if (collegeTotalNpv > directNpv) {
                recommendationHtml = `
                    <p style="color: #28a745; font-weight: bold;">📌 强烈推荐：优先选择读大学！</p>
                    <p>读大学的总净现值（NPV）比直接工作高 <span class="highlight">${(collegeTotalNpv - directNpv).toFixed(2)} 元</span>，长期收益更优。</p>
                    <p>关键原因：毕业后薪资增长带来的长期回报远超过直接工作的短期现金流，且大学期间的奖学金+兼职收入部分抵消了成本。（注意：计算结果因相关参数不同可能出现较大差异，数据仅供参考）</p>
                `;
            } else {
                recommendationHtml = `
                    <p style="color: #dc3545; font-weight: bold;">⚠️ 建议：优先选择直接工作。</p>
                    <p>直接工作的总净现值（NPV）比读大学高 <span class="highlight">${(directNpv - collegeTotalNpv).toFixed(2)} 元</span>，短期现金流更稳定。</p>
                    <p>关键原因：大学期间的成本+机会成本较高，且毕业后薪资增长的幅度不足以覆盖前期投入。（注意：计算结果因相关参数不同可能出现较大差异，数据仅供参考）</p>
                `;
            }


            // 8. 展示结果
            document.getElementById('directNpv').textContent = directNpv.toLocaleString('zh-CN');
            document.getElementById('collegeNpv').textContent = collegeTotalNpv.toLocaleString('zh-CN');
            document.getElementById('recommendation').innerHTML = recommendationHtml;
            document.getElementById('result').style.display = 'block';
        }
    </script>
</body>
</html>
