# LivePhotosStore - Bootstrap5實作
 
# 安裝Bootstrap以修改特定樣式

1. 為了能夠修改Bootstrap原先設定的樣式，需透過下載Bootstrap裡的scss檔案
2. 在專案中安裝Bootstrap的方式有以下兩種：
    1. 使用npm方式
    2. [官網](https://getbootstrap.com/docs/5.0/getting-started/download/)上下載原始碼

npm方式：

1. 創建專案資料夾，例如叫myproj
2. 開啟終端機，cd到myproj資料夾
3. 輸入指令 npm i bootstrap

官網下載方式：

1. 進入[下載網頁](https://getbootstrap.com/docs/5.0/getting-started/download/)
2. 在Source files段落裡點選Download source按鈕
3. 下載後解壓縮檔案
4. 在資料夾裡面找到SCSS資料夾，將此資料夾改名後(自訂)，並加入myproj專案中使用

## 修改樣式

1. 尋找到此路徑的scss/_variable.scss檔案，Bootstrap中所有使用到的變數都在此檔案中
2. 若想修改/新增Bootstrap中會使用到有關color色系的class樣式，可透過以下

    ```scss
    // scss-docs-start theme-colors-map
    $theme-colors: (
    "primary": $primary,
    "secondary": $secondary,
    "success": $success,
    "info": $info,
    "warning": $warning,
    "danger": $danger,
    "light": $light,
    "dark": $dark,
    ) !default;
    ```

    依照設計稿的主色及常用的顏色，故修改primary的顏色，並新增black1-5的顏色，如此在執行scss後，即會自動產生該色系的一系列樣式，ex：text-black1、bg-black1等...。

    ```scss
    $theme-colors: (
      "primary":    #FFDF65,
      "secondary":  $secondary,
      "success":    $success,
      "info":       $info,
      "warning":    $warning,
      "danger":     $danger,
      "light":      $light,
      "dark":       $dark,
      "black1":   #494846,
      "black2":   #636057,
      "black3":   #858377,
      "black4":   #373A3C,
      "black5":   #F7F7F7
    ) !default;
    ```

3. 若想修改tooltip樣式，可修改以下內容

    ```scss
    $tooltip-color:                     #FFDF65 !default;
    $tooltip-bg:                        #494846 !default;
    ```

4. 字型
    1. 設計稿中針對中文字是使用NotoSansCJKtc，數字與英文字符號是使用Baloo Tamma 2
        1. NotoSansCJKtc字體：

            NotoSansCJKtc可至此[下載路徑](https://briian.com/25754/)，並將會使用到的字型的otf檔加入到專案資料夾myproj/font，並在scss資料夾新增_font.scss，將字型引入如下

            ```scss
            @font-face {
                font-family: NotoSansCJKtc-Bold;
                src: url(../font/NotoSansCJKtc-Bold.otf);
            }
            @font-face {
                font-family: NotoSansCJKtc-Regular;
                src: url(../font/NotoSansCJKtc-Regular.otf);
            }
            @font-face {
                font-family: NotoSansCJKtc-Medium;
                src: url(../font/NotoSansCJKtc-Medium.otf);
            }
            ```

        2. Baloo Tamma 2字體：

            Baloo Tamma 2可至[Google Fonts](https://fonts.google.com/)中選定字體後，取得import的路徑，並將此路徑新增至_font.scss，如下：

            ```scss
            @import url('https://fonts.googleapis.com/css2?family=Baloo+Tamma+2:wght@400;600&display=swap');
            ```

    2. 為了後續方便設定元素字型，故直接單獨把字體獨立成class來使用

        ```scss
        .f-regular {
            font-family: "NotoSansCJKtc-Regular";
        }

        .f-medium {
            font-family: "NotoSansCJKtc-Medium";
        }

        .f-bold {
            font-family: "NotoSansCJKtc-Bold";
        }

        .f-baloo-regular {
            font-family: "Baloo Tamma 2";
            font-weight:400;
        }

        .f-baloo-blod {
            font-family: "Baloo Tamma 2";
            font-weight:600;
        }
        ```

    ## 專案實作內容

    ### header

    1. 首先導覽列的部分是使用Bootstrap中的Navbar元件，因設計稿上並非滿版設計，Bootstrap範例的Navbar的內容設定為滿版，故會將內層的<div class="container-fluid"> 修改為 <div class="container">，如此也能滿足設計稿中的背景及下方的邊框線為滿版。
    2. Logo的部分會加在Navbar元件結構中的.navbar-brand，並透過background屬性引入Logo圖片，並透過圖片覆蓋文字方式，將Logo設定寬高後定位至指定的位置

        ```scss
        .navbar-brand {
            display: block;
            width: 111px;
            height: 40px;
            background: url(../img/logo.jpg);
            background-size: cover;
        		//圖片取代文字的方式
            text-indent: 101%;
            white-space: nowrap;
            overflow: hidden;
            margin-right: 24px;
        }
        ```

    3. 在Navbar右側加入兩顆按鈕，為了讓ul li的列表與右側的button分離，且讓button靠Navbar右側，會在ul標籤中加入me-auto，按鈕的形狀設定參考通用類別的邊框(Borders)的rounded-pill樣式
    4. 登入的button是參考Bootstrap元件中的Modal互動視窗
    5. Navbar底端的邊框線設定可參考通用類別的[邊框(Borders)](https://bootstrap5.hexschool.com/docs/5.0/utilities/borders/)

    ## header-subcontent

    1. 第二區塊包括上方標題、下方左側區塊、下方右側內容
    2. 上方標題是本網頁中最重要的資訊，故會使用h1標籤，而下方的內容是採用Bootstrap格線系統，左邊為7、右邊為5的比例進行分配。
    3. 右下方的內容中，標章列表使用ul li結構，針對hover效果是套用Bootstrap的[工具提示框 (Tooltips)](https://bootstrap5.hexschool.com/docs/5.0/components/tooltips/)
    4. 在加入Tooltips元件後，需在javascript裡加上以下幾行以進行tooltips初始化：

        ```jsx
        // 初始化tooltips
        var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'));
        var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
        	return new bootstrap.Tooltip(tooltipTriggerEl);
        });
        ```

    ## fixbar 頁籤列表

    1. 此區塊的頁籤是使用Bootstrap的[導覽與頁籤 (Navs and tabs)](https://bootstrap5.hexschool.com/docs/5.0/components/navs-tabs/)，透過頁籤來抽換下面的內容，這邊有兩種做法，可以把抽換內容做在同一頁，也可以將其分別做成四頁用href連結來變換內容，這邊是採用前者做法。
    2. 為了實現當滑鼠卷軸往下滾動時，navs & tabs 頁籤往上置頂的效果，有以下兩種做法
        1. 在navs & tabs 在外層加上[.sticky-top](https://bootstrap5.hexschool.com/docs/5.0/helpers/position/#sticky-top)類別
        2. 在navs & tabs 在外層加上position-sticky，並加入手寫樣式z-index 999
    3. 頁籤選取當下的底線樣式

        ```scss
        .nav-item .active {
            font-family: "NotoSansCJKtc-Bold";
            border-bottom: 2px solid #FFDF65;
        }
        ```

    ## content內容

    1. navs & tabs 下方對應的內容也是使用格線系統，這邊以「專案介紹」為例，切分如下：
        1. 左方專案介紹區塊：佔8欄
        2. 右方sidebar贊助卡片區塊：佔4欄
    2. 為符合設計稿指定此兩區塊間格48px，故在.row身上加入gutter(g-5)
    3. 因為fixbar加上了navs & tabs功能，若要在點擊頁先後對應的內容飛出，則需加上以下類別
        1. 在每個主題最外層都須加上.tab-pane
        2. 加上 .fade 實現飛出效果
        3. 若預設先顯示該區塊內容，則加上 .show
        4. 加上id屬性，並對照到上方的頁籤連結href
    4. 「常見問題」區塊：
        1. 使用Bootstrap的[手風琴 (Accordion)](https://bootstrap5.hexschool.com/docs/5.0/components/accordion/)元件，內容因為都為同類別，故採用ul li結構。
        2. 若要修改選取每項問題，產生黃色邊框，則需修改以下的_variable.scss內容

            ```scss
            $accordion-button-active-bg:              $body-bg !default;
            $accordion-button-active-color:           shade-color($primary, 100%) !default;
            ```

    5. 、「目前進度」區塊：
        1. 使用ul li結構，每個li上加上Bootstrap的[card元件](https://bootstrap5.hexschool.com/docs/5.0/components/card/)
        2. 在card元件中，左邊圖片右邊內容，圖片與內容使用格線系統，為了配合圖片的寬度，左側圖片區塊設定 .col-auto，右邊內容區塊設定.col，如此及配合圖片大小來分配兩區塊的範圍
        3. 因card元件中的左邊圖片區塊與右邊內容區塊之間並無保留空間，固可加入gutter(g-0)
    6. 、「留言」區塊：

        設定上與「目前進度」區塊雷同，留言者資訊(照片、名字、留言時間)部分使用格線系統分配

    ## Sidebar區塊

    1. sidebar區塊內容會加入.position-sticky，實現外區塊卷軸移至sidebar區塊時sidebar置頂；加入.overflow-scroll類別，並設定內容高度height: 100vh;，使內層內容大於螢幕高度而溢出時透過卷軸方式來呈現內容
    2. 原本設定電腦版sidebar高度為一個螢幕高，變換成手機版時，若忘記更改設定，則sidebar內容會超出原先設定的100vh的設定範圍，會產生下一個區塊內容被壓在「sidebar中超出範圍的內容」之下，故設定手機版樣式時須加上以下：

        ```scss
        @include mobile {
            height: auto;
        }
        ```

    ## 最下方的贊助Form表單區塊

    最上方的愛心圖與正下方的贊助專案標題設定如下：

    1. 加上.text-center將愛心圖置中
    2. 專案贊助標題：

        ```html
        <h2 class="caption fs-4 text-black1 text-center mb-4">贊助專案</h2>
        ```

        ```scss
        .caption {
            position: relative;
            display: block;
        }
        .caption::before, .caption::after {
            content: "";
            position: absolute;
            top: 12px;
            background-color: #FFDF65;
            width: calc((100% - 136px)/2);
            height: 3px;
        }
        .caption::before {
            left: 0px;
        }
        .caption::after {
            right: 0px;
        }
        ```

        備註：記得在calc運算時，要在運算符號左右保留空白，避免無法計算報錯

    3. Form表單區塊-表格元件
        1. 這邊是使用Bootstrap的[表單 (Forms)](https://bootstrap5.hexschool.com/docs/5.0/forms/overview/)與[驗證 (Validation)](https://bootstrap5.hexschool.com/docs/5.0/forms/validation/)
        2. 每個Form元件都須記得加入form-* 的 class，否則整個表單內容無法對齊
        3. 在「表格元件」與「表格的項目標籤」中都會個別加上id 與 for 屬性來對應，如下：

            ```html
            <label for="validationName" class="form-label text-black4 mb-2">收件人姓名</label>
            <input type="text" class="form-control" id="validationName" required>
            ```

            如此使用者在輸入資料時，只要點選項目名稱，即會自動選取到要輸入的指定物件，此部分與Bootstrap無關

    4. Form表單區塊-資料驗證
        1. 主要是在form元素上加上.needs-validation類別， 並加入novalidate屬性，已取消預設的HTML5表單驗證
        2. 在input與select加上required屬性，表示該欄位必填
        3. 在最後的button按鈕加入type="submit"屬性，並在js檔案加上以下(Bootstrap)程式碼

            ```jsx
            // 表單驗證
            // Example starter JavaScript for disabling form submissions if there are invalid fields
            (function () {
            'use strict';

            // Fetch all the forms we want to apply custom Bootstrap validation styles to
            var forms = document.querySelectorAll('.needs-validation');

            // Loop over them and prevent submission
            Array.prototype.slice.call(forms).forEach(function (form) {
                form.addEventListener('submit', function (event) {
                    if (!form.checkValidity()) {
                    event.preventDefault();
                    event.stopPropagation();
                    }

                    form.classList.add('was-validated');
                }, false)
                })
            })()
            ```

    ## JS代碼部分

    為了實現以下三個功能，故透過jQuery語法進行撰寫，功能與程式碼如下：

    1. 位移到Nav對應的內容

        ```jsx
        function goToNav() {
            var windowWidth = $(window).width();
            if(windowWidth <=576) {
                $("html, body").scrollTop(948);
            }else {
                $("html, body").scrollTop(810);
            }
        }
        ```

    2. 位移到Form對應的內容

        ```jsx
        function goToForm() {
            var windowWidth = $(window).width();
            if(windowWidth <=576) {
                $("html, body").scrollTop(3865);
            }else {
                $("html, body").scrollTop(2479);
            }
        }
        ```

    3. 切換到手機版時，在規定的卷軸高度出現/消失滿版的「贊助專案按鈕」

        ```jsx
        $(window).scroll(function() {
            var windowWidth = $(window).width();
            var scrollHeight = $(window).scrollTop();
            
            if (windowWidth <= 576) {
                if(scrollHeight >=948 && scrollHeight <= 3748) {
                    if($(".floatBtn").hasClass("d-none")) {
                        $(".floatBtn").removeClass("d-none");
                        $(".floatBtn").addClass("d-block");
                    }
                }else {
                    if($(".floatBtn").hasClass("d-block")) {
                        $(".floatBtn").removeClass("d-block");
                        $(".floatBtn").addClass("d-none");
                    }
                }
            }
        });

        let a = document.querySelectorAll("a");
        a.forEach(function(item){
            item.addEventListener("click",function(e){
                e.preventDefault();
            });
        });
        ```
