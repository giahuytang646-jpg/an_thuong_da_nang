import os
import json
import zipfile

# ============================
# Tạo thư mục agent chuẩn
# ============================
base = "/mnt/data/agent_export_fixed"
intents_dir = os.path.join(base, "intents")
os.makedirs(intents_dir, exist_ok=True)

# ============================
# agent.json (chuẩn 100% Dialogflow ES)
# ============================
agent_data = {
    "description": "",
    "language": "vi",
    "shortDescription": "",
    "supportedLanguages": [],
    "defaultTimezone": "Asia/Ho_Chi_Minh",
    "enabledKnowledgeBaseNames": [],
    "googleAssistant": {
        "googleAssistantCompatible": False,
        "project": "",
        "welcomeIntentId": "",
        "startIntents": [],
        "endIntentIds": [],
        "oAuthLinking": {
            "required": False
        },
        "systemIntents": [],
        "voiceType": "MALE_1"
    },
    "webhook": {
        "url": "",
        "username": "",
        "password": "",
        "available": True
    },
    "isPrivate": True,
    "mlMinConfidence": 0.3,
    "mlUseAuto": True
}

with open(os.path.join(base, "agent.json"), "w", encoding="utf-8") as f:
    json.dump(agent_data, f, indent=4, ensure_ascii=False)

# ============================
# Intent: giới thiệu an thượng
# ============================
intent_name = "gioi_thieu_an_thuong"
intent_response = {
    "name": intent_name,
    "auto": True,
    "responses": [{
        "messages": [{
            "lang": "vi",
            "speech": [
                "Khu phố du lịch An Thượng nằm ở quận Ngũ Hành Sơn.",
                "Khu vực này nổi tiếng sôi động, có nhiều dịch vụ cho khách quốc tế."
            ]
        }]
    }]
}

intent_usersays = [
    {"data": [{"text": "an thượng là gì"}], "isTemplate": False},
    {"data": [{"text": "giới thiệu về khu phố an thượng"}], "isTemplate": False},
    {"data": [{"text": "an thượng có gì nổi bật"}], "isTemplate": False},
]

with open(os.path.join(intents_dir, f"{intent_name}.json"), "w", encoding="utf-8") as f:
    json.dump(intent_response, f, indent=4, ensure_ascii=False)

with open(os.path.join(intents_dir, f"{intent_name}_usersays_vi.json"), "w", encoding="utf-8") as f:
    json.dump(intent_usersays, f, indent=4, ensure_ascii=False)

# ============================
# Tạo ZIP
# ============================
zip_path = "/mnt/data/dialogflow_agent_FIXED.zip"

with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as zipf:
    zipf.write(os.path.join(base, "agent.json"), "agent.json")
    for file in os.listdir(intents_dir):
        zipf.write(os.path.join(intents_dir, file), f"intents/{file}")

zip_path

