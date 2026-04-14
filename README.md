const {
  Document, Packer, Paragraph, TextRun, AlignmentType,
  HeadingLevel, LevelFormat, BorderStyle, UnderlineType
} = require('docx');
const fs = require('fs');

const rtl = true;

function heading1(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_1,
    bidirectional: rtl,
    alignment: AlignmentType.RIGHT,
    spacing: { before: 320, after: 160 },
    children: [
      new TextRun({
        text,
        font: "Arial",
        size: 32,
        bold: true,
        color: "1A3C6E",
        rtl,
      })
    ]
  });
}

function heading2(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_2,
    bidirectional: rtl,
    alignment: AlignmentType.RIGHT,
    spacing: { before: 240, after: 120 },
    children: [
      new TextRun({
        text,
        font: "Arial",
        size: 26,
        bold: true,
        color: "2E5FA3",
        rtl,
      })
    ]
  });
}

function body(text) {
  return new Paragraph({
    bidirectional: rtl,
    alignment: AlignmentType.RIGHT,
    spacing: { before: 80, after: 80 },
    children: [
      new TextRun({
        text,
        font: "Arial",
        size: 22,
        rtl,
      })
    ]
  });
}

function bullet(label, desc) {
  const children = [];
  if (label) {
    children.push(new TextRun({ text: label + ": ", font: "Arial", size: 22, bold: true, rtl }));
  }
  children.push(new TextRun({ text: desc, font: "Arial", size: 22, rtl }));
  return new Paragraph({
    bidirectional: rtl,
    alignment: AlignmentType.RIGHT,
    spacing: { before: 60, after: 60 },
    indent: { right: 360 },
    children: [
      new TextRun({ text: "• ", font: "Arial", size: 22, rtl }),
      ...children
    ]
  });
}

function emptyLine() {
  return new Paragraph({
    bidirectional: rtl,
    children: [new TextRun({ text: "", size: 22 })]
  });
}

const doc = new Document({
  styles: {
    default: {
      document: {
        run: { font: "Arial", size: 22, rtl: true },
        paragraph: { bidirectional: true, alignment: AlignmentType.RIGHT }
      }
    },
    paragraphStyles: [
      {
        id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial", color: "1A3C6E" },
        paragraph: { spacing: { before: 320, after: 160 }, outlineLevel: 0, bidirectional: true, alignment: AlignmentType.RIGHT }
      },
      {
        id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 26, bold: true, font: "Arial", color: "2E5FA3" },
        paragraph: { spacing: { before: 240, after: 120 }, outlineLevel: 1, bidirectional: true, alignment: AlignmentType.RIGHT }
      }
    ]
  },
  sections: [{
    properties: {
      page: {
        size: { width: 11906, height: 16838 },
        margin: { top: 1440, right: 1800, bottom: 1440, left: 1800 }
      },
      bidi: true,
    },
    children: [
      // Title
      new Paragraph({
        bidirectional: rtl,
        alignment: AlignmentType.CENTER,
        spacing: { before: 0, after: 200 },
        border: {
          bottom: { style: BorderStyle.SINGLE, size: 8, color: "1A3C6E", space: 10 }
        },
        children: [
          new TextRun({
            text: "سياسة الخصوصية لتطبيق Antra",
            font: "Arial",
            size: 48,
            bold: true,
            color: "1A3C6E",
            rtl,
          })
        ]
      }),

      // Date
      new Paragraph({
        bidirectional: rtl,
        alignment: AlignmentType.CENTER,
        spacing: { before: 100, after: 400 },
        children: [
          new TextRun({
            text: "آخر تحديث: 14 أبريل 2026",
            font: "Arial",
            size: 20,
            color: "666666",
            italics: true,
            rtl,
          })
        ]
      }),

      // Intro
      body("تصف سياسة الخصوصية هذه كيفية قيام تطبيق Antra بجمع المعلومات المتعلقة بك واستخدامها ومشاركتها عند استخدامك لتطبيقنا على الهاتف المحمول."),

      emptyLine(),

      // Section 1
      heading1("١. المعلومات التي نجمعها"),

      heading2("أ. المعلومات التي تقدمها أنت"),
      bullet("معلومات الحساب", 'عند إنشاء حساب جديد، قد نجمع عنوان بريدك الإلكتروني ومعلومات الملف الشخصي الأخرى المقدَّمة عبر مزوّد المصادقة (Supabase).'),
      bullet("تقدم التعلم", "نجمع البيانات المتعلقة بنشاطك التعليمي، بما في ذلك محاولات التدريب، والكلمات التي أتقنتها، ومعدلات الدقة، وسلاسل الإنجاز. تُستخدم هذه البيانات لتزويدك بالإحصائيات وتتبع تقدمك."),
      bullet("محتوى المستخدم", "قد نجمع المعلومات التي تدخلها في التطبيق، مثل تفاعلات الدردشة أو التفضيلات."),

      emptyLine(),
      heading2("ب. المعلومات المجمَّعة تلقائياً"),
      body("عند استخدامك للتطبيق، قد نجمع تلقائياً بعض المعلومات، تشمل:"),
      bullet("معلومات الجهاز", "نجمع معلومات عن جهازك المحمول، بما في ذلك طراز الجهاز، ونظام التشغيل وإصداره، والمعرّفات الفريدة للجهاز، ومعلومات شبكة الهاتف المحمول."),
      bullet("معلومات الاستخدام", "نجمع معلومات حول نشاطك على التطبيق، مثل الميزات التي تستخدمها والوقت الذي تقضيه فيه."),

      emptyLine(),

      // Section 2
      heading1("٢. خدمات الطرف الثالث"),
      body("نستخدم خدمات طرف ثالث قد تجمع معلومات تُستخدم للتعرف عليك:"),

      heading2("أ. Supabase"),
      body("نستخدم Supabase لخدمات المصادقة وقواعد البيانات. قد تجمع Supabase معلومات وفقاً لسياسة الخصوصية الخاصة بها."),

      heading2("ب. Google Mobile Ads (AdMob)"),
      body("نستخدم إعلانات Google Mobile Ads لعرض الإعلانات. قد تستخدم Google معرّفات الإعلانات (مثل IDFA على نظام iOS أو معرّف الإعلانات على نظام Android) لتقديم إعلانات مخصصة."),

      heading2("ج. Expo"),
      body("يتم تطوير التطبيق باستخدام إطار عمل Expo. قد تجمع Expo بعض المعلومات التشخيصية كجزء من بنيتها التحتية."),

      emptyLine(),

      // Section 3
      heading1("٣. كيفية استخدامنا لمعلوماتك"),
      body("نستخدم المعلومات التي نجمعها من أجل:"),
      bullet("", "توفير التطبيق وصيانته وتحسينه."),
      bullet("", "مصادقة المستخدمين وإدارة الحسابات."),
      bullet("", "عرض الإعلانات المخصصة (إن وُجدت)."),
      bullet("", "الرد على تعليقاتك وأسئلتك وطلباتك."),
      bullet("", "مراقبة الاتجاهات والاستخدام والأنشطة المتعلقة بتطبيقنا وتحليلها."),

      emptyLine(),

      // Section 4
      heading1("٤. خياراتك والتحكم في بياناتك"),

      heading2("أ. الإعلانات"),
      body("يمكنك إلغاء الاشتراك في الإعلانات المبنية على الاهتمامات عن طريق ضبط إعدادات جهازك المحمول:"),
      bullet("iOS", 'انتقل إلى الإعدادات > الخصوصية > التتبع، وقم بتعطيل "السماح للتطبيقات بطلب التتبع".'),
      bullet("Android", 'انتقل إلى الإعدادات > Google > الإعلانات، وقم بتفعيل "إلغاء الاشتراك في تخصيص الإعلانات".'),

      emptyLine(),
      heading2("ب. بيانات الحساب"),
      body("يمكنك طلب حذف حسابك والبيانات المرتبطة به عن طريق التواصل معنا أو من خلال الإعدادات المتاحة داخل التطبيق."),

      emptyLine(),

      // Section 5
      heading1("٥. الأمان"),
      body("نتخذ تدابير معقولة للمساعدة في حماية معلوماتك من الفقدان والسرقة وسوء الاستخدام والوصول غير المصرح به والإفصاح والتعديل والتدمير."),

      emptyLine(),

      // Section 6
      heading1("٦. خصوصية الأطفال"),
      body("لا يستهدف تطبيقنا الأطفال دون سن 13 عاماً. نحن لا نجمع عن قصد معلومات شخصية من الأطفال الذين تقل أعمارهم عن 13 عاماً."),

      emptyLine(),

      // Section 7
      heading1("٧. التغييرات على هذه السياسة"),
      body("قد نقوم بتحديث سياسة الخصوصية هذه من وقت لآخر. إذا أجرينا تغييرات، سنُعلمك بذلك عن طريق تعديل التاريخ في أعلى السياسة."),

      emptyLine(),

      // Section 8
      heading1("٨. اتصل بنا"),
      body("إذا كان لديك أي أسئلة حول سياسة الخصوصية هذه، يُرجى التواصل معنا على: [بريدك الإلكتروني / رابط التواصل]"),

      emptyLine(),

      // Footer note
      new Paragraph({
        bidirectional: rtl,
        alignment: AlignmentType.CENTER,
        spacing: { before: 400, after: 0 },
        border: {
          top: { style: BorderStyle.SINGLE, size: 4, color: "CCCCCC", space: 10 }
        },
        children: [
          new TextRun({
            text: "© 2026 Antra. جميع الحقوق محفوظة.",
            font: "Arial",
            size: 18,
            color: "888888",
            rtl,
          })
        ]
      }),
    ]
  }]
});

Packer.toBuffer(doc).then(buffer => {
  fs.writeFileSync("/mnt/user-data/outputs/Antra_Privacy_Policy_AR.docx", buffer);
  console.log("Done!");
});
