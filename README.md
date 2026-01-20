import streamlit as st
import plotly.graph_objects as go

# ===============================
# FinAI Advisor – Executive Engine
# Interactive Multi-Layer Visualization
# ===============================

st.set_page_config(page_title="FinAI Advisor", layout="wide")
st.title("🧠 FinAI Advisor – Executive Decision Engine")
st.markdown("Interactive Multi-Layer Visualization + AI-Supported Decisions")
st.markdown("From Accounting Entry ➜ Mandatory Executive Decision")

# ---------- 1️⃣ Scenarios ----------
SCENARIOS = {
    "1️⃣ Accounting Profit + Negative Cash": {"sales_credit": 100000, "cash_collected": 20000, "expenses_paid": 60000, "customers_late_ratio": 0.45},
    "2️⃣ Good Cash + Operational Loss": {"sales_credit": 50000, "cash_collected": 60000, "expenses_paid": 80000, "customers_late_ratio": 0.10},
    "3️⃣ Poor Collection + High Sales": {"sales_credit": 150000, "cash_collected": 30000, "expenses_paid": 70000, "customers_late_ratio": 0.60},
    "4️⃣ High Expenses": {"sales_credit": 80000, "cash_collected": 70000, "expenses_paid": 90000, "customers_late_ratio": 0.20},
    "5️⃣ Stable Financials": {"sales_credit": 100000, "cash_collected": 95000, "expenses_paid": 60000, "customers_late_ratio": 0.05}
}

scenario_name = st.selectbox("📂 Select Scenario", SCENARIOS.keys())
data = SCENARIOS[scenario_name]

# ---------- 2️⃣ Engine ----------
def validate_data(data):
    missing = [k for k, v in data.items() if v is None]
    return (len(missing) == 0), missing

def financial_impact(data):
    cash_flow = data["cash_collected"] - data["expenses_paid"]
    accounting_profit = data["sales_credit"] - data["expenses_paid"]
    return cash_flow, accounting_profit

def diagnose(cash_flow, profit, late_ratio):
    if profit > 0 and cash_flow < 0:
        return "Liquidity Problem"
    if late_ratio > 0.4:
        return "Collection Problem"
    if profit <= 0:
        return "Profitability Problem"
    return "Stable"

def executive_decision(problem):
    main_decisions = {
        "Liquidity Problem": "🔴 Stop credit sales and enforce collections immediately",
        "Collection Problem": "🔴 Freeze overdue clients and activate collection plan",
        "Profitability Problem": "🔴 Reduce operating costs immediately",
        "Stable": "🟢 Maintain strategy with monitoring"
    }
    supporting_decisions = {
        "Liquidity Problem": ["Review supplier payments schedule", "Accelerate cash inflows"],
        "Collection Problem": ["Send reminders to clients", "Offer early payment discounts"],
        "Profitability Problem": ["Optimize operating costs", "Negotiate better rates with vendors"],
        "Stable": ["Continue monitoring KPIs", "Plan next quarter strategy"]
    }
    improvement_recommendations = {
        "Liquidity Problem": ["Explore short-term financing"],
        "Collection Problem": ["Implement automated collection system"],
        "Profitability Problem": ["Review pricing strategy"],
        "Stable": ["Regular internal audits"]
    }
    return main_decisions[problem], supporting_decisions[problem], improvement_recommendations[problem]

def risk_layer(problem):
    risks = {
        "Liquidity Problem": {"Type":"Financial","Level":"High","Action":"Immediate intervention"},
        "Collection Problem": {"Type":"Financial","Level":"High","Action":"Immediate intervention"},
        "Profitability Problem": {"Type":"Operational","Level":"High","Action":"Cost reduction"},
        "Stable": {"Type":"Operational","Level":"Low","Action":"Routine monitoring"}
    }
    return risks[problem]

def governance_layer():
    return {"Decision Owner":"Executive Management","Review Cycle":"Weekly","Escalation":"Board if not executed"}

def strategic_impact(problem):
    impact = {
        "Liquidity Problem":["Improve liquidity","Reduce financial risk","Stabilize operations"],
        "Collection Problem":["Faster collections","Lower overdue","Improve cash flow"],
        "Profitability Problem":["Reduce costs","Increase profitability","Optimize operations"],
        "Stable":["Maintain stability","Monitor KPIs","Prepare strategic growth"]
    }
    return impact[problem]

def ai_recommendations(data):
    recs=[]
    if data["customers_late_ratio"]>0.4: recs.append("Consider predictive reminders for late-paying clients")
    if (data["expenses_paid"]/data["sales_credit"])>0.7: recs.append("Analyze high expense ratios for cost optimization")
    if data["cash_collected"]<data["expenses_paid"]: recs.append("Plan short-term financing or accelerate collections")
    if data["sales_credit"]-data["expenses_paid"]<0: recs.append("Review pricing and revenue recognition strategy")
    if not recs: recs.append("Operations look stable; continue monitoring KPIs")
    return recs

# ---------- 3️⃣ MNEE Simulation ----------
def mnee_simulation(main_dec, supporting_dec):
    simulated_actions = []
    if "Stop credit sales" in main_dec:
        simulated_actions.append("✅ Credit sales stopped in system")
    if "Freeze overdue clients" in main_dec:
        simulated_actions.append("✅ Overdue client accounts frozen")
    if "Reduce operating costs" in main_dec:
        simulated_actions.append("✅ Operating costs reduced")
    if "Maintain strategy" in main_dec:
        simulated_actions.append("✅ Strategy maintained and monitored")
    for action in supporting_dec:
        simulated_actions.append(f"✅ {action} executed digitally")
    return simulated_actions

# ---------- Run Engine ----------
if st.button("▶️ Run Executive Engine"):
    valid, missing = validate_data(data)
    if not valid:
        st.error(f"Missing data: {missing}")
    else:
        cash_flow, profit = financial_impact(data)
        problem = diagnose(cash_flow, profit, data["customers_late_ratio"])
        main_dec, supporting_dec, improvement = executive_decision(problem)
        risks = risk_layer(problem)
        governance = governance_layer()
        strategy = strategic_impact(problem)
        ai_recs = ai_recommendations(data)
        mnee_actions = mnee_simulation(main_dec, supporting_dec)

        # ---------- Display Data ----------  
        st.subheader("📥 Financial Data")  
        st.json(data)  

        st.subheader("📊 Financial Impact")  
        st.write(f"Cash Flow: {cash_flow}")  
        st.write(f"Accounting Profit: {profit}")  

        st.subheader("🧠 Diagnosis")  
        st.write(problem)  

        st.subheader("🎯 Decisions")  
        st.success(main_dec)  
        st.write("🟠 Supporting:", supporting_dec)  
        st.write("🟢 Recommendations:", improvement)  

        st.subheader("⚠️ Risk Management")  
        st.json(risks)  

        st.subheader("🏛️ Governance")  
        st.json(governance)  

        st.subheader("🌍 Strategic Impact")  
        st.write(strategy)  

        st.subheader("🤖 AI Recommendations")  
        st.write(ai_recs)  

        st.subheader("💰 MNEE – Simulated Digital Transactions")  
        for act in mnee_actions:
            st.write(act)

        # ---------- Enhanced Interactive Layer Visualization ----------
        layers = [
            "Input Data","Data Validation","Financial Impact","Diagnosis",
            "Indicators","Decision Engine","Risk Management","Governance","Strategic Impact","MNEE – Digital Transactions"
        ]
        hover_texts = [
            f"Data Entered: {data}",
            "All data validated ✅",
            f"Cash Flow: {cash_flow}, Profit: {profit}",
            f"Diagnosis: {problem}",
            "Key Indicators Calculated",
            f"Main Decision: {main_dec}\nSupporting: {supporting_dec}\nImprovement: {improvement}",
            f"Risk: {risks}",
            f"Governance: {governance}",
            f"Strategic Impact: {strategy}\nAI Recommendations: {ai_recs}",
            "Simulated digital transactions executed via MNEE"
        ]
        values = [1]*10
        colors = ["#636EFA","#648FFF","#00CC96","#AB63FA","#FFA15A","#19D3F3","#FF6692","#B6E880","#FF97FF","#FFD700"]

        fig = go.Figure(go.Bar(
            x=values,
            y=layers,
            orientation='h',
            marker_color=colors,
            text=layers,
            hovertext=hover_texts,
            hoverinfo="text",
            textposition='inside',
            opacity=0.9
        ))
        fig.update_layout(
            height=650,
            yaxis={'categoryorder':'array','categoryarray':layers},
            title="FinAI Advisor – Multi-Layer Executive Engine",
            title_font_size=22,
            plot_bgcolor="#f9f9f9",
            paper_bgcolor="#f9f9f9",
            margin=dict(l=20, r=20, t=50, b=20)
        )

        st.plotly_chart(fig, use_container_width=True)
