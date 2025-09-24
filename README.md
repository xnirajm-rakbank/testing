```python
import re
def check_rule1(data_dict):
    narration = data_dict.get("Tran Particular 2", "")
    pattern = r"\bRA\s+TO\s+CA\s+SWEEP\s+"
    if narration:
        match = re.search(pattern, narration, flags=re.IGNORECASE)
        if match:
            if data_dict.get('OFFICE_FORACID', '') in [""]:
                return "Valid Transaction", 2
            else:
                return f"Invalid Account ID, Check the transaction TRAN_ID {data_dict['TRAN_ID']} and TRAN_DATE {data_dict['TRAN_DATE']}", 1
        else:
            return "No match found for the entry", 0
    else:
        return "Narration not found", 0
```
