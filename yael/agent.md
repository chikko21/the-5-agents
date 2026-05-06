# Yael — Content Writer Agent (יעל)

תיקיית עבודה של הסוכנת יעל. ההגדרה הקנונית של הסוכן: [`.claude/agents/yael.md`](../.claude/agents/yael.md).

## מה יש כאן

| נתיב | תפקיד |
|------|-------|
| `style-guide.md` | מדריך הסגנון שיעל קוראת בתחילת **כל** סשן. הקול, הקצב, מבנה המאמר, מה להימנע ממנו. |
| `reference/` | דוגמאות לטקסטים בסגנון שלנו (Markdown או טקסט). יעל סורקת את התיקייה ומשתמשת בהן כקלט סגנוני. כשהיא ריקה — יעל עובדת רק לפי `style-guide.md`. |

## איך זה משתלב בארכיטקטורה

```
Content/<name>.md         ← מאמר גלם (קלט)
   │
   ▼
yael (LLM-only)           ← קוראת style-guide + reference, משכתבת
   │
   ├──→ Output/<name>.md           ← תוצר עם {{IMAGE_NEEDED}} placeholders
   └──→ Content/Ready/<name>.md    ← עותק לארכיון (המקור נשאר ב-Content/)
   │
   ▼ (אם יש placeholders)
Jacob → yuval → Jacob ערוך תוצר סופי
```

יעל היא **LLM-only**: יש לה רק `Read`, `Write`, `Edit`, `Glob`, `Grep`. אין לה `Bash`, `Task`, או גישה לרשת. היא לא מפעילה את יובל ישירות — היא משאירה placeholders ש-Jacob פותר.

## תחזוקה של `style-guide.md` ו-`reference/`

- **שינוי קול הפרויקט**: ערוך את `style-guide.md`. השינוי תקף מהסשן הבא של יעל.
- **הוספת דוגמה חדשה**: שמור קובץ `.md` או `.txt` תחת `reference/`. לא נדרש שינוי בקוד — יעל מגלה אותו ב-`Glob`.
- **אל תוסיף קבצים מסוגים אחרים** ל-`reference/` (תמונות, JSON, קוד). יעל לא תקרא אותם נכון.
