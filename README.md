# Đếm Slick
<?php

    if (count($slider)) {
        $countItems = count($slider);
        echo '<span> <label class="numb-slick">1</label> / ' . $countItems . '</span>';
    }

?>

# Translate
<div class="translate">
    <?php
    $params = array();
    $params['act'] = '1';
    echo $func->mymarkdown('translate/translate', $params);
    ?>
</div>

<div class="translate">
    <?php
    $params = array();
    $params['act'] = '1';
    echo $func->mymarkdown('translate/translate', $params);
    ?>
</div>

# Tiktok

<?php
// Tiktok functions

function getTiktok($tikTokURL)
{
    preg_match('/\/video\/(\d+)$/', $tikTokURL, $matches);
    return isset($matches[1]) ? $matches[1] : null;
}

function TikTokPro($url = 'https://www.tiktok.com/', $height = '300')
{
    $parts = explode('@', $url);
    $username = end($parts);
    $embedCode = '<blockquote class="tiktok-embed p-0 m-0 d-inline-block" cite="https://www.tiktok.com/' . $username . '" data-unique-id="' . $username . '" data-embed-type="creator" style="max-width: 500px; min-width: 288px;height: ' . $height . 'px"></blockquote>';
    return $embedCode;
}
?>

# Lock

<?php
// Lock page script

$todayPage = time();
$intTimeLock = strtotime('20-02-2024');

if ($intTimeLock < $todayPage) {
    header("Location:lock.php");
    exit;
}

// hoặc

$intTimeLock = strtotime('03-07-2024');

if ($intTimeLock < $todayPage) {
    echo "<script>
            alert('Chức năng đang bảo trì !!!');
            window.location.href = '../';
          </script>";
    exit;
}
?>

# Flipbook

<script>
document.addEventListener('DOMContentLoaded', function() {
    var flipbookEL = document.getElementById('flipbook');
    
    function updateFlipbookSize() {
        if (window.innerWidth <= 768) {
            $(flipbookEL).turn({
                autoCenter: true,
                page: 2,
                height: 280,
            });
        } else {
            $(flipbookEL).turn({
                autoCenter: true,
                page: 2,
                height: 730,
            });
        }
    }

    window.addEventListener('resize', function(e) {
        flipbookEL.style.width = '';
        flipbookEL.style.height = '';
        $(flipbookEL).turn('size', flipbookEL.clientWidth, flipbookEL.clientHeight);
        updateFlipbookSize(); // Cập nhật kích thước của flipbook khi thay đổi kích thước cửa sổ
    });

    updateFlipbookSize(); // Gọi hàm để cài đặt kích thước ban đầu

    $(".turn-next").click(function(event) {
        $("#flipmenu").turn("next");
    });

    $(".turn-prev").click(function(event) {
        $("#flipmenu").turn("previous");
    });

    // Đếm slick
    $('.slick-items').on('afterChange', function(event, slick, currentSlide) {
        $('.numb-slick').text(currentSlide + 1);
    });
});
</script>
