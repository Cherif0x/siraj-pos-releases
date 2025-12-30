# دليل النشر السريع - سراج

## للمطورين فقط

هذا دليل سريع لإصدار نسخة جديدة من البرنامج.

---

## 🎯 الهدف

إنشاء نسخة جاهزة للتوزيع من برنامج سراج.

---

## 📋 المتطلبات

- ✅ .NET SDK مُثبّت
- ✅ (اختياري) Inno Setup لإنشاء ملف التثبيت

---

## 🚀 الطريقة الموصى بها (خطوة واحدة)

### تشغيل السكربت التلقائي

```powershell
# افتح PowerShell في مجلد المشروع
cd "C:\SIRAJE CSharp"

# شغّل السكربت
.\PublishAndBuild.ps1
```

### ماذا يفعل السكربت؟

1. ✅ يقرأ رقم الإصدار من `SIRAJE-Setup.iss`
2. ✅ ينشر البرنامج إلى مجلد `publish_versions\<version>`
3. ✅ ينسخ الملفات إلى مجلد `publish\`
4. ✅ يحاول إنشاء ملف التثبيت (إن وُجد Inno Setup)

---

## 🔧 الطريقة اليدوية

### 1. نشر البرنامج

```powershell
# نشر البرنامج
dotnet publish InventoryManagement.csproj `
    -c Release `
    -r win-x64 `
    --self-contained true `
    -o publish_versions\1.1.1
```

### 2. نسخ الملفات

```powershell
# مسح مجلد publish القديم
Remove-Item publish\* -Recurse -Force

# نسخ الملفات الجديدة
Copy-Item publish_versions\1.1.1\* publish\ -Recurse
```

### 3. إنشاء ملف التثبيت (اختياري)

```powershell
# إذا كان Inno Setup مثبت
ISCC.exe SIRAJE-Setup.iss
```

---

## 📦 هيكل المجلدات

```
C:\SIRAJE CSharp\
├── publish\                  ← الإصدار الحالي (يقرأه Inno Setup)
├── publish_versions\         ← أرشيف جميع الإصدارات
│   ├── 1.0.0\
│   ├── 1.1.0\
│   └── 1.1.1\               ← الإصدار الأحدث
└── SIRAJE-Setup.iss         ← ملف إعداد Inno Setup
```

---

## ✅ التحقق من النشر

### 1. تحقق من الملفات

```powershell
# عرض محتويات مجلد publish
dir publish\
```

**الملفات المطلوبة:**
- ✅ `InventoryManagement.exe`
- ✅ `Resources\` (مجلد)
- ✅ `app.ico`

### 2. اختبر البرنامج

```powershell
# شغّل البرنامج
cd publish
.\InventoryManagement.exe
```

**المتوقع:**
- ✅ البرنامج يفتح بدون أخطاء
- ✅ شاشة التفعيل تظهر

---

## 📊 معلومات مهمة

### حجم البرنامج

- **المجلد المنشور:** ~250 MB
- **ملف التثبيت (ZIP):** ~200 MB
- **ملف Inno Setup:** ~150 MB

### البرنامج Self-Contained

> ℹ️ **ملاحظة:**  
> البرنامج يحتوي على جميع المكتبات المطلوبة.  
> العميل لا يحتاج تثبيت .NET Runtime.

---

## 🔄 تحديث رقم الإصدار

### في ملف المشروع

**ملف:** `InventoryManagement.csproj`

```xml
<Version>1.1.1</Version>
```

### في ملف Inno Setup

**ملف:** `SIRAJE-Setup.iss`

```
#define MyAppVersion "1.1.1"
```

> ⚠️ **مهم:** تأكد من تطابق رقم الإصدار في الملفين!

---

## 📝 ملاحظات

### الإصدارات السابقة

- ✅ محفوظة في `publish_versions\<version>`
- ✅ لا تُحذف تلقائياً
- ✅ يمكن الرجوع إليها عند الحاجة

### ملف التثبيت

- ✅ يُنشأ في مجلد `Output\`
- ✅ الاسم: `SIRAJE-Setup-v1.1.1.exe`

---

## 🆘 حل المشاكل

### "dotnet command not found"

**الحل:** ثبّت .NET SDK من موقع Microsoft

### "ISCC.exe not found"

**الحل:** ثبّت Inno Setup أو تجاهل هذه الخطوة

### "Access denied"

**الحل:** شغّل PowerShell كمسؤول (Run as Administrator)

---

## 📞 الدعم

إذا واجهت مشكلة:
- راجع ملف `DEPLOYMENT_GUIDE.md` للتفاصيل الكاملة
- تواصل مع فريق التطوير

---

**آخر تحديث:** ديسمبر 2025  
**الإصدار:** v1.1.1
