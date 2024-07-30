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

# Counter

<script>
    if (isExist($('#counter'))) {
    var a = 0;
    $(window).scroll(function() {
        var oTop = $('#counter').offset().top - window.innerHeight;
        if (a === 0 && $(window).scrollTop() > oTop) {
            $('.counter-value').each(function() {
                var $this = $(this),
                    countTo = $this.attr('data-count');
                $({
                    countNum: $this.text()
                }).animate({
                    countNum: countTo
                },
                {
                    duration: 1000,
                    easing: 'swing',
                    step: function() {
                        $this.text(Math.floor(this.countNum));
                    },
                    complete: function() {
                        $this.text(this.countNum);
                    }
                });
            });
            a = 1;
        }
    });
}


    # Mail
    <!-- Send email -->
$subject = "Thư liên hệ từ " . $setting['name' . $lang];
$message = str_replace($emailVars, $emailVals, $emailer->markdown('contact/admin'));
if ($emailer->send("admin", null, $subject, $message, 'file_attach')) {
    $func->transfer("Gửi liên hệ thành công", $configBase);
} else {
    $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau", $configBase, false);
}
} else {
    $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau", $configBase, false);
}

<!-- Save data -->
if (($scoreCaptcha >= 0.5 && $actionCaptcha == 'Newsletter') || $testCaptcha === true) {
    $data = array(
        'email' => htmlspecialchars($dataNewsletter['email']),
        'phone' => htmlspecialchars($dataNewsletter['phone']),
        'fullname' => htmlspecialchars($dataNewsletter['fullname']),
        'content' => htmlspecialchars($dataNewsletter['content']),
        'date_created' => time(),
        'type' => 'dangkynhantin'
    );

    if ($d->insert('newsletter', $data)) {
        $id_insert = $d->getLastInsertId();
        if ($func->hasFile("file_attach")) {
            $file_name = $func->uploadName($_FILES['file_attach']["name"]);
            $allowedExtensions = '.doc|.docx|.pdf|.rar|.zip|.ppt|.pptx|.DOC|.DOCX|.PDF|.RAR|.ZIP|.PPT|.PPTX|.xls|.xlsx|.jpg|.png|.gif|.JPG|.PNG|.GIF';
            if ($file_attach = $func->uploadImage("file_attach", $allowedExtensions, UPLOAD_FILE_L, $file_name)) {
                $fileUpdate = array('file_attach' => $file_attach);
                $d->where('id', $id_insert);
                $d->update('newsletter', $fileUpdate);
            }
        }
        $func->transfer("Đăng ký nhận tin thành công. Chúng tôi sẽ liên hệ với bạn sớm.", $configBase);
    } else {
        $func->transfer("Đăng ký nhận tin thất bại. Vui lòng thử lại sau.", $configBase, false);
    }
} else {
    $func->transfer("Đăng ký nhận tin thất bại. Vui lòng thử lại sau.", $configBase, false);
}

/* Newsletter */
if (!empty($_POST['submit-newsletter'])) {
    $responseCaptcha = $_POST['recaptcha_response_newsletter'];
    $resultCaptcha = $func->checkRecaptcha($responseCaptcha);
    $scoreCaptcha = (!empty($resultCaptcha['score'])) ? $resultCaptcha['score'] : 0;
    $actionCaptcha = (!empty($resultCaptcha['action'])) ? $resultCaptcha['action'] : '';
    $testCaptcha = (!empty($resultCaptcha['test'])) ? $resultCaptcha['test'] : false;
    $dataNewsletter = (!empty($_POST['dataNewsletter'])) ? $_POST['dataNewsletter'] : null;

    $error = $flash->get('error');

    if (!empty($error)) {
        $func->transfer($error, $configBase, false);
    }

    /* Save data */
    if (($scoreCaptcha >= 0.5 && $actionCaptcha == 'Newsletter') || $testCaptcha == true) {
        $data = array();
        $data['fullname'] = htmlspecialchars($dataNewsletter['fullname']);
        $data['phone'] = htmlspecialchars($dataNewsletter['phone']);
        $data['email'] = htmlspecialchars($dataNewsletter['email']);
        $data['date_created'] = time();
        $data['type'] = 'dangkynhantin';

        if ($d->insert('newsletter', $data)) {
            $id_insert = $d->getLastInsertId();

            if ($func->hasFile("file_attach")) {
                $fileUpdate = array();
                $file_name = $func->uploadName($_FILES['file_attach']["name"]);

                if ($file_attach = $func->uploadImage("file_attach", '.doc|.docx|.pdf|.rar|.zip|.ppt|.pptx|.DOC|.DOCX|.PDF|.RAR|.ZIP|.PPT|.PPTX|.xls|.xlsx|.jpg|.png|.gif|.JPG|.PNG|.GIF', UPLOAD_FILE_L, $file_name)) {
                    $fileUpdate['file_attach'] = $file_attach;
                    $d->where('id', $id_insert);
                    $d->update('contact', $fileUpdate);
                    unset($fileUpdate);
                }
            }
        } else {
            $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau.", $configBase, false);
        }

        /* Gán giá trị gửi email */
        $strThongtin = '';
        $emailer->set('tennguoigui', $data['fullname']);
        $emailer->set('emailnguoigui', $data['email']);
        $emailer->set('dienthoainguoigui', $data['phone']);
        if ($emailer->get('tennguoigui')) {
            $strThongtin .= '<span style="text-transform:capitalize"> Tên : ' . $emailer->get('tennguoigui') . '</span><br>';
        }

        if ($emailer->get('emailnguoigui')) {
            $strThongtin .= 'Email: ' . $emailer->get('emailnguoigui') . '<br>';
        }

        if ($emailer->get('dienthoainguoigui')) {
            $strThongtin .= 'SĐT: ' . $emailer->get('dienthoainguoigui') . '<br>';
        }
    
      
        $emailer->set('thongtin', $strThongtin);

        /* Defaults attributes email */
        $emailDefaultAttrs = $emailer->defaultAttrs();

        /* Variables email */
        $emailVars = array(
            '{emailTitleSender}',
            '{emailInfoSender}',
            '{emailSubjectSender}',
            '{emailContentSender}'
        );
        $emailVars = $emailer->addAttrs($emailVars, $emailDefaultAttrs['vars']);

        /* Values email */
        $emailVals = array(
            $emailer->get('tennguoigui'),
            $emailer->get('thongtin'),
            $emailer->get('tieudelienhe'),
            $emailer->get('noidunglienhe')
        );
        $emailVals = $emailer->addAttrs($emailVals, $emailDefaultAttrs['vals']);

        /* Send email admin */
        $arrayEmail = null;
        $subject = "Thư liên hệ từ " . $setting['name' . $lang];
        $message = str_replace($emailVars, $emailVals, $emailer->markdown('contact/admin'));
        $file = 'file_attach';

        if ($emailer->send("admin", $arrayEmail, $subject, $message, $file)) {
            /* Send email customer */
            $arrayEmail = array(
                "dataEmail" => array(
                    "name" => $emailer->get('tennguoigui'),
                    "email" => $emailer->get('emailnguoigui')
                )
            );
            $subject = "Thư liên hệ từ " . $setting['name' . $lang];
            $message = str_replace($emailVars, $emailVals, $emailer->markdown('contact/customer'));
            $file = 'file_attach';

            if ($emailer->send("customer", $arrayEmail, $subject, $message, $file)) $func->transfer("Gửi liên hệ thành công", $configBase);
        } else $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau", $configBase, false);
    } else {
        $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau", $configBase, false);
    }
}


/* Booking */
if (!empty($_POST['submit-newsletter2'])) {
    $responseCaptcha = $_POST['recaptcha_response_newsletter'];
    $resultCaptcha = $func->checkRecaptcha($responseCaptcha);
    $scoreCaptcha = (!empty($resultCaptcha['score'])) ? $resultCaptcha['score'] : 0;
    $actionCaptcha = (!empty($resultCaptcha['action'])) ? $resultCaptcha['action'] : '';
    $testCaptcha = (!empty($resultCaptcha['test'])) ? $resultCaptcha['test'] : false;
    $dataNewsletter = (!empty($_POST['dataNewsletter'])) ? $_POST['dataNewsletter'] : null;

    $error = $flash->get('error');

    if (!empty($error)) {
        $func->transfer($error, $configBase, false);
    }

     /* Save data */
     if (($scoreCaptcha >= 0.5 && $actionCaptcha == 'Newsletter') || $testCaptcha == true) {
        $data = array();
        $data['fullname'] = htmlspecialchars($dataNewsletter['fullname']);
        $data['phone'] = htmlspecialchars($dataNewsletter['phone']);
        $data['content'] = htmlspecialchars($dataNewsletter['content']);
        $data['address'] = htmlspecialchars($dataNewsletter['address']);
        $data['day'] = htmlspecialchars($dataNewsletter['day']);
        $emailer->set('emailnguoigui', $data['email']);
        $data['date_created'] = time();
        $data['type'] = 'dangkybaogia';

        if ($d->insert('newsletter', $data)) {
            $id_insert = $d->getLastInsertId();

            if ($func->hasFile("file_attach")) {
                $fileUpdate = array();
                $file_name = $func->uploadName($_FILES['file_attach']["name"]);

                if ($file_attach = $func->uploadImage("file_attach", '.doc|.docx|.pdf|.rar|.zip|.ppt|.pptx|.DOC|.DOCX|.PDF|.RAR|.ZIP|.PPT|.PPTX|.xls|.xlsx|.jpg|.png|.gif|.JPG|.PNG|.GIF', UPLOAD_FILE_L, $file_name)) {
                    $fileUpdate['file_attach'] = $file_attach;
                    $d->where('id', $id_insert);
                    $d->update('contact', $fileUpdate);
                    unset($fileUpdate);
                }
            }
        } else {
            $func->transfer("Gửi liên hệ thất bại. Vui lòng thử lại sau.", $configBase, false);
        }

        /* Gán giá trị gửi email */
        $strThongtin = '';
        $emailer->set('tennguoigui', $data['fullname']);
        $emailer->set('emailnguoigui', $data['email']);
        $emailer->set('daynguoigui', $data['day']);
        $emailer->set('diachinguoigui', $data['address']);
        $emailer->set('dienthoainguoigui', $data['phone']);
        $emailer->set('noidunglienhe', $data['content']);
      
        if ($emailer->get('tennguoigui')) {
            $strThongtin .= '<span style="text-transform:capitalize"> Tên : ' . $emailer->get('tennguoigui') . '</span><br>';
        }
        if ($emailer->get('emailnguoigui')) {
            $strThongtin .= 'Email : ' . $emailer->get('emailnguoigui') . '<br>';
        }

        if ($emailer->get('dienthoainguoigui')) {
            $strThongtin .= 'SĐT: ' . $emailer->get('dienthoainguoigui') . '<br>';
        }
        if ($emailer->get('daynguoigui')) {
            $strThongtin .= 'Thời gian : ' . $emailer->get('daynguoigui') . '<br>';
        }
        if ($emailer->get('diachinguoigui')) {
            $strThongtin .= 'Địa chỉ : ' . $emailer->get('diachinguoigui') . '<br>';
        }
        if ($emailer->get('noidunglienhe')) {
            $strThongtin .= 'Nội dung : ' . $emailer->get('noidunglienhe') . '<br>';
        }
        $emailer->set('thongtin', $strThongtin);

        /* Defaults attributes email */
        $emailDefaultAttrs = $emailer->defaultAttrs();

        /* Variables email */
        $emailVars = array(
            '{emailTitleSender}',
            '{emailInfoSender}',
            '{emailSubjectSender}',
            '{emailContentSender}'
        );
        $emailVars = $emailer->addAttrs($emailVars, $emailDefaultAttrs['vars']);

        /* Values email */
        $emailVals = array(
            $emailer->get('tennguoigui'),
            $emailer->get('thongtin'),
            $emailer->get('tieudelienhe'),
            $emailer->get('noidunglienhe')
        );
        $emailVals = $emailer->addAttrs($emailVals, $emailDefaultAttrs['vals']);

        /* Send email admin */
        $arrayEmail = null;
        $subject = "Thư liên hệ từ " . $setting['name' . $lang];
        $message = str_replace($emailVars, $emailVals, $emailer->markdown('contact/admin'));
        $file = 'file_attach';

        if ($emailer->send("admin", $arrayEmail, $subject, $message, $file)) {
            $func->transfer("Gửi đặt bàn thành công", $configBase);
        } else $func->transfer("Gửi đặt bàn thất bại. Vui lòng thử lại sau", $configBase, false);
    } else {
        $func->transfer("Gửi đặt bàn thất bại. Vui lòng thử lại sau", $configBase, false);
    }
}
?>


</script>


# Text-shadow
text-shadow: 0px 0px 0 var(--color-main-v2), 2px -1px 0 var(--color-main-v2), -1px 3px 0 var(--color-main-v2), 2px 2px 0 var(--color-main-v2);
