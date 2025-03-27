<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>القصص</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
</head>
<body>
  <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            direction: rtl;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            padding: 10px;
            width: 100%;
        }

        .app-bar {
            width: 100%;
            background: linear-gradient(135deg, #007bff, #0056b3);
            color: white;
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            padding: 10px 0;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 1000;
        }

        .search-box {
            width: 90%;
            max-width: 400px;
            margin-top: 60px;
            display: flex;
            align-items: center;
            background: white;
            padding: 8px;
            border-radius: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .search-box input {
            flex: 1;
            border: none;
            outline: none;
            padding: 8px;
            font-size: 16px;
            border-radius: 20px;
        }

        .search-box i {
            margin-left: 10px;
            color: #007bff;
        }

        .container {
            width: 95vw;
            max-width: 600px;
            margin-top: 20px;
        }

        .story-box {
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 10px;
            padding: 10px;
            display: flex;
            align-items: center;
            cursor: pointer;
            transition: 0.3s;
        }

        .story-box:hover {
            background: #f1f1f1;
        }

        .story-box img {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            margin-left: 10px;
            object-fit: cover;
        }

        .story-box h3 {
            font-size: 16px;
            color: #333;
            margin: 0;
        }
  </style>

  <div class="app-bar">📖 قائمة القصص</div>

  <!-- 🔎 خانة البحث -->
  <div class="search-box">
      <i class="fas fa-search"></i>
      <input type="text" id="searchInput" placeholder="ابحث عن قصة...">
  </div>

  <div class="container" id="storyList">
      <!-- يتم توليد القصص هنا ديناميكيًا -->
  </div>

  <script>
        function getCategoryFromURL() {
            const params = new URLSearchParams(window.location.search);
            return params.get("category") || "default";
        }

        // بيانات القصص لكل قسم
        const stories = {
            q1: [
                { title: "الأرنب السريع", image: "https://via.placeholder.com/60", file: "rabbit.html" },
                { title: "السلحفاة الحكيمة", image: "https://via.placeholder.com/60", file: "turtle.html" }
            ],
            q2: [
                { title: "قصة آدم عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "adam.html" },
{ title: "قصة إدريس عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "idris.html" },
{ title: "قصة نوح عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "noah.html" },
{ title: "قصة هود عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "hud.html" },
{ title: "قصة صالح عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "saleh.html" },
{ title: "قصة إبراهيم عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "ibrahim.html" },
{ title: "قصة لوط عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "lut.html" },
{ title: "قصة إسماعيل عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "ismail.html" },
{ title: "قصة إسحاق عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "ishaq.html" },
{ title: "قصة يعقوب عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "yaqoob.html" },
{ title: "قصة يوسف عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "yusuf.html" },
{ title: "قصة أيوب عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "ayyoub.html" },
{ title: "قصة شعيب عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "shuaib.html" },
{ title: "قصة موسى عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "mosa.html" },
{ title: "قصة هارون عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "haroon.html" },
{ title: "قصة ذو الكفل عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "zulkifl.html" },
{ title: "قصة داود عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "dawud.html" },
{ title: "قصة سليمان عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "sulaiman.html" },
{ title: "قصة إلياس عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "ilyas.html" },
{ title: "قصة اليسع عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "alyasa.html" },
{ title: "قصة يونس عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "yunus.html" },
{ title: "قصة زكريا عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "zakariya.html" },
{ title: "قصة يحيى عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "yahya.html" },
{ title: "قصة عيسى عليه السلام", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "isa.html" },
{ title: "قصة محمد صلى الله عليه وسلم", image: "https://i.postimg.cc/7Lf8NfsQ/open-book-1.png", file: "muhammad.html" }
            ],
            q3: [
                { title: "سيف الله خالد بن الوليد", image: "https://via.placeholder.com/60", file: "khalid.html" }
            ],
            q6: [
                { title: "المنزل المسكون", image: "https://via.placeholder.com/60", file: "haunted.html" },
                { title: "الدمية الملعونة", image: "https://via.placeholder.com/60", file: "doll.html" }
            ],
            default: []
        };

        // عرض القصص بناءً على القسم
        function displayStories() {
            const category = getCategoryFromURL();
            const storyList = document.getElementById("storyList");
            const storyData = stories[category] || [];

            storyList.innerHTML = "";

            if (storyData.length === 0) {
                storyList.innerHTML = "<p>❌ لا توجد قصص متاحة لهذا القسم</p>";
                return;
            }

            storyData.forEach(story => {
                const storyBox = document.createElement("div");
                storyBox.classList.add("story-box");
                storyBox.innerHTML = `
                    <img src="${story.image}" alt="${story.title}">
                    <h3>${story.title}</h3>
                `;
                storyBox.addEventListener("click", () => {
                    window.location.href = story.file;
                });
                storyList.appendChild(storyBox);
            });
        }

        // البحث عن القصص
        document.getElementById("searchInput").addEventListener("input", function () {
            const searchText = this.value.toLowerCase();
            const storyBoxes = document.querySelectorAll(".story-box");

            storyBoxes.forEach(box => {
                const title = box.querySelector("h3").innerText.toLowerCase();
                box.style.display = title.includes(searchText) ? "flex" : "none";
            });
        });

        // تحميل القصص عند فتح الصفحة
        displayStories();
  </script>

</body>
</html>
