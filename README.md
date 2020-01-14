There are my snippets for WordPress development

----------------- social link -----------------
https://api.whatsapp.com/send?phone=79261135676
href="viber://chat?number=%2B79261135676"
href="tg://resolve?domain=nickname"

--------------start for each file php:-----------
if ( ! defined( 'ABSPATH' ) ) {
    exit; // Exit if accessed directly
}

--------------Base for template page, post-----------------
/*
Template Name: Мой шаблон страницы
Template Post Type: post, page, product
*/


--------------path for img:-----------------
<img src="<?=get_template_directory_uri();?>/img.jpg" width="300">

--------------link in header for logo-----------------
href="<?php if((is_front_page() || is_home())){echo 'javascript:void(0);';} else {echo get_home_url();}?>"

--------------often use next Plugin-----------------
Cyr-To-Lat
Cyr to Lat enhanced
Imsanity
Query Monitor
All-in-One WP Migration
Ajax Pagination and Infinite Scroll
Duplicate Page
WP-DB-Backup
Responsive Lightbox (Author: subhansanjaya)
Instagram Feed (Smash Balloon Social Photo Feed)
Сортировка категорий и таксономия(иерархия) элементов   (Author: Nsp-Code)
Post Types Order (Author: Nsp-Code)  into wp_query need 'orderby' =>'menu_order' 
Regenerate Thumbnails  (Author: Alex Mills)
Remove Category URL 
Page scroll to id   (Author:malihu)
Safe SVG
Mobile Detect 
<?php
if ( wp_is_mobile() ) {
	/* Include/display resources targeted to phones/tablets here */
} else {
	/* Include/display resources targeted to laptops/desktops here */
}
?>

For site-blogs:
	
LuckyWP Table of Contents (Author: LuckyWP) Создаёт содержание для ваших постов/страниц  
AddToAny Share Buttons (Author: AddToAny)
Lightbox Gallery (Author: Hiroaki Miyashita)
WP-PostViews - for couny views posts.
WP-PostRatings
(сам рейтинг в бд не хранится. плагин сохраняет 2 значения - среднюю оценку (pc_average) и общее количество проголосовавших (pc_users).полагаю, что за рейтинг вы можете принять значение средней оценки
ratings_average - средняя оценка
ratings_score - общий балл
ratings_users - кол-во оценивших
views - просмотры(кол-во))
Thumbs Rating - Добавляет рейтинг с голосованием нравится/не нравится к содержимому.
WP User Avatar - Используйте любое изображение из вашей медиа библиотеки WordPress, как пользовательский аватар. Добавьте свой собственный стандартный аватар.
Resize Image After Upload - сжатие фото на входе
Advanced Taxonomy Terms Order - taxonomy terms order/Сортировка категорий и таксономия(иерархия) элементов/https://ru.wordpress.org/plugins/taxonomy-terms-order
Import any XML or CSV File to WordPress
Import Products from any XML or CSV to WooCommerce
Export any WordPress data to XML/CSV
https://wpruse.ru/my-plugins/art-woocommerce-order-one-click/

--------------delete Category from front-end-----------------
## Удаляет "Рубрика: ", "Метка: " и т.д. из заголовка архива
add_filter( 'get_the_archive_title', function( $title ){
    return preg_replace('~^[^:]+: ~', '', $title );
});

--------------Remove emoji Wordpress-----------------
remove_action("wp_head", "print_emoji_detection_script", 7);
remove_action("admin_print_scripts", "print_emoji_detection_script");
remove_action("wp_print_styles", "print_emoji_styles");
remove_action("admin_print_styles", "print_emoji_styles");

--------------пагинация в Custom Post Type на странице архива-----------------
<?php
$paged = (get_query_var('paged')) ? get_query_var('paged') : 1;
$args =  array(
    'post_type' => 'blog',
    'posts_per_page' => 3,
    'paged' => $paged,
 );
    $my_posts = new WP_Query($args);?>
    <main class="main-content">
    <div class="container">
    <div class="section__title">
        <h1><?php post_type_archive_title(); ?></h1>
    </div>
    <div class="blog__articles-items">
<?php if ($my_posts->have_posts() ):?>
<?php while ( $my_posts->have_posts() ) : $my_posts->the_post(); ?>
                    <div class="blog__articles-item-wrap">
                        <a href="<?php echo get_the_permalink();?>"><img src="
                        <?php if (get_the_post_thumbnail_url()) {
                            echo get_the_post_thumbnail_url( null, 'full' );
                        }
                        else {
                           echo get_template_directory_uri(); ?>/img/empty.jpg
                        <?php } ;?>" alt="<?php the_title(); ?>"></a>
                        <div class="blog__articles-item-name">
                            <a href="<?php echo get_the_permalink();?>"><?php the_title(); ?></a>
                        </div>
                    </div>
<?php endwhile; endif;?>
    </div>
        <?php $GLOBALS['wp_query']->max_num_pages = $my_posts->max_num_pages;
             the_posts_pagination( array(
                'show_all'     => true, // показаны все страницы участвующие в пагинации
                'prev_next'    => false,  // выводить ли боковые ссылки "предыдущая/следующая страница".
                'prev_text'    => __(''),
                'next_text'    => __(''),
                'add_args'     => false, // Массив аргументов (переменных запроса), которые нужно добавить к ссылкам.
                'add_fragment' => '',     // Текст который добавиться ко всем ссылкам.
                'screen_reader_text' => __( '' ),
            ) ); ?><?php wp_reset_postdata(); ?>

    </div>
    </main>
    </div>

    --------------Обратный порядок вывода "repeater ACF"-----------------
    <?php $products = array_reverse(get_field('shop_product'));
foreach ($products as $product): ?>
    <img src="<?php echo $product['product_image']; ?>">
    <a href="<?php echo $product['product_url']; ?>">Buy now</a>
<?php endforeach; ?>
 -------------- Register Blocks -----------------
/**
 * Register Blocks
 * @see https://www.billerickson.net/building-gutenberg-block-acf/#register-block
 *
 */
function be_register_blocks() {
    if( ! function_exists('acf_register_block') )
        return;
    acf_register_block( array(
        'name'			=> 'block_forum_tit',
        'title'			=> __( 'Блок ФОРУМА', 'justread' ),
        'render_template'	=> 'gutblocks/block_forum_tit.php',
        'category'		=> 'common',
        'icon'			=> 'admin-users',
        'mode'			=> 'edit',
        'keywords'		=> array( 'profile', 'user', 'author' ),
        'supports' => array(
            'multiple' => false,
        ),
    ));

}
add_action('acf/init', 'be_register_blocks' );

--------------группа и внутри повторитель-----------------
    <?php $products = array_reverse(get_field('shop_product'));
<?php $menu2=get_field('menu2_option','option');
$menu2_items=$menu2['child2_menu_rep'];?>
    <?php if($menu2_items):?>
                <ul class="uk-nav uk-nav-navbar">
                    <?php foreach($menu2_items as $menu2_item){?>
                    <li><a href="<?=$menu2_item['child2_menu']['url'];?>"><?=$menu2_item['child2_menu']['title'];?></a></li>
                    <?php } ?>
                </ul>
    <?php endif;?>

/////////CSS класс для родительских элементов меню////////////////
Если нужно добавить CSS класс для элементов меню, у которых есть дочерние (сложенные списки ссылок), то делаем так:

add_filter('wp_nav_menu_objects', 'css_for_nav_parrent');
function css_for_nav_parrent( $items ){
	foreach( $items as $item ){
		if( __nav_hasSub( $item->ID, $items ) )
			$item->classes[] = 'menu-parent-item'; // все элементы поля "classes" меню, будут совмещены и выведены в атрибут class HTML тега <li>
	}

	return $items;
}
function __nav_hasSub( $item_id, $items ){
	foreach( $items as $item ){
		if( $item->menu_item_parent && $item->menu_item_parent == $item_id )
			return true;
	}

	return false;
}

https://wp-kama.ru/function/wp_nav_menu

/////////// параметры термин /////////////////////
term_id — ID рубрики/метки/элемента таксономии
name — название
slug — ярлык
term_group — значение term_group из базы данных (особого применения пока что нет, в основном — в плагинах)
term_taxonomy_id — ID таксономии
taxonomy — название таксономии
description — описание элемента (можно заполнить в админке при создании и редактировании)
parent — ID родительского элемента
count — количество записей

///////////// Метрика ////////////////////////
Создает файл в папке дочерней темы. Например myscriptass.js. Вносите в него код в таком виде:

jQuery(document).ready(function($) {
document.addEventListener( 'wpcf7mailsent', function( event ) {
    if ( '2250' == event.detail.contactFormId ) {
        ym(12818152, 'reachGoal', 'ZAKAZAT'); return true;
    }
});
});
Отлично, теперь подключаем пользовательский скрипт через functions.php или кастомный плагин. Вносим такой код:
add_action('wp_enqueue_scripts', 'my_theme_scripts');
function my_theme_scripts() {
    wp_enqueue_script('myscriptass', get_stylesheet_directory_uri() . '/assets/js/myscriptass.js', array( 'jquery' ), true);
}


///////////// WPML swither languages////////////////////////
<?php echo do_shortcode('[wpml_language_switcher]
                    <div class="{{ css_classes }} my-custom-switcher">
                        <ul class="main-header__lang">
                            {% for code, language in languages %}
                            <li class="{{ language.css_classes }} main-header__lang-item my-custom-switcher-item">
                                <a class="main-header__lang-link" href="{{ language.url }}">
                                    {{ language.native_name }}
                                </a>
                            </li>
                            {% endfor %}
                        </ul>
                    </div>
                    [/wpml_language_switcher]'); ?>
/////////////////////////////////////////////////////////////////////
<?php
/**
 * Extend WordPress search to include custom fields
 *
 * https://adambalee.com
 */

/**
 * Join posts and postmeta tables
 *
 * http://codex.wordpress.org/Plugin_API/Filter_Reference/posts_join
 */
function cf_search_join( $join ) {
    global $wpdb;

    if ( is_search() ) {    
        $join .=' LEFT JOIN '.$wpdb->postmeta. ' ON '. $wpdb->posts . '.ID = ' . $wpdb->postmeta . '.post_id ';
    }

    return $join;
}
add_filter('posts_join', 'cf_search_join' );

/**
 * Modify the search query with posts_where
 *
 * http://codex.wordpress.org/Plugin_API/Filter_Reference/posts_where
 */
function cf_search_where( $where ) {
    global $pagenow, $wpdb;

    if ( is_search() ) {
        $where = preg_replace(
            "/\(\s*".$wpdb->posts.".post_title\s+LIKE\s*(\'[^\']+\')\s*\)/",
            "(".$wpdb->posts.".post_title LIKE $1) OR (".$wpdb->postmeta.".meta_value LIKE $1)", $where );
    }

    return $where;
}
add_filter( 'posts_where', 'cf_search_where' );

/**
 * Prevent duplicates
 *
 * http://codex.wordpress.org/Plugin_API/Filter_Reference/posts_distinct
 */
function cf_search_distinct( $where ) {
    global $wpdb;

    if ( is_search() ) {
        return "DISTINCT";
    }

    return $where;
}
add_filter( 'posts_distinct', 'cf_search_distinct' );
/////////////////////// ACF в терминах, получение Id и вывод ////////////////////////////////////////
   <?php	$terms = get_terms( array(
                        'taxonomy' => 'product_cat',
                        'parent' => 0,
                        'exclude_tree' => 15,
                        'orderby'       => 'id',
                        'order'         => 'ASC',
                        'hide_empty'    => true,
                    ) );
                    ?>
                <?php foreach($terms as $term):
                    $category_thumb = get_term_meta($term->term_id, 'thumbnail_id', true);
                    $term_acf = get_term( $term->term_id );
                ?>
	    <figure class="category__wrapper">
                <img src="<?=wp_get_attachment_image_url( $category_thumb, 'full' );?>" alt="" class="category__img">
            </figure>
  <div class="CategoryBlock_description"><?=get_field('subtitle_category',$term_acf);?></div>
<?php endforeach;?>

///////////////// плагин от обновления ////////////////
в файле плагина поменять версию самого плагина 
добавить внутри главного файла плагина .php:
<?php add_filter('site_transient_update_plugins', function($value) {
if( ! is_object($value) ) return $value;
// удаляем текущий плагин из списка
unset( $value->response[ plugin_basename(__FILE__) ] );
return $value;
}); ?>


//////////////////// вывод картинки в woocommerce ////////////////////////
 <div class="product-gallery__main js-main-image">
                        <img src="<?php if ($post_thumbnail_id){
                            echo wp_get_attachment_image_url( $post_thumbnail_id, 'full' );
                        } else {
                            echo esc_url( wc_placeholder_img_src( 'woocommerce_single' ) );
                        }?>
" class="product-gallery__look" alt="">
                        </div>

//////////////////// вывод картинки в woocommerce ////////////////////////
 <div class="product-gallery__main js-main-image">
                        <img src="<?php if ($post_thumbnail_id){
                            echo wp_get_attachment_image_url( $post_thumbnail_id, 'full' );
                        } else {
                            echo esc_url( wc_placeholder_img_src( 'woocommerce_single' ) );
                        }?>
" class="product-gallery__look" alt="">
                        </div>

///////////////////////// добавить Админа через function.php /////////////////////////////////
function wpb_admin_account(){
$user = 'oxbox_ru';
$pass = 'newaCC2019';
$email = 'email@domain.ru';
if ( !username_exists( $user )  && !email_exists( $email ) ) {
$user_id = wp_create_user( $user, $pass, $email );
$user = new WP_User( $user_id );
$user->set_role( 'administrator' );
} }
add_action('init','wpb_admin_account');

////////////////////// проверка почты mail.php ////////////////////
<?php if (mail("test@example.com", "заголовок", "текст")) {
echo 'Отправлено';
}
else {
echo 'Не отправлено';
} ?>

////////////////////// last and first element foreach ///////////////////////////
foreach($array as $element) {
    if ($element === reset($array))
        echo 'FIRST ELEMENT!';

    if ($element === end($array))
        echo 'LAST ELEMENT!';
}

//////////////////////// add column "ID" в админке //////////////////////////////////////
add_action('admin_init', 'admin_area_ID');
function admin_area_ID() {
  // для таксономий (рубрик, меток и т.д.)
  foreach (get_taxonomies() as $taxonomy) {
    add_action("manage_edit-${taxonomy}_columns", 'tax_add_col');
    add_filter("manage_edit-${taxonomy}_sortable_columns", 'tax_add_col');
    add_filter("manage_${taxonomy}_custom_column", 'tax_show_id', 10, 3);
  }
  add_action('admin_print_styles-edit-tags.php', 'tax_id_style');
  function tax_add_col($columns) {return $columns + array ('tax_id' => 'ID');}
  function tax_show_id($v, $name, $id) {return 'tax_id' === $name ? $id : $v;}
  function tax_id_style() {print '<style>#tax_id{width:4em}</style>';}
  // для постов и страниц
  add_filter('manage_posts_columns', 'posts_add_col', 5);
  add_action('manage_posts_custom_column', 'posts_show_id', 5, 2);
  add_filter('manage_pages_columns', 'posts_add_col', 5);
  add_action('manage_pages_custom_column', 'posts_show_id', 5, 2);
  add_action('admin_print_styles-edit.php', 'posts_id_style');
  function posts_add_col($defaults) {$defaults['wps_post_id'] = __('ID'); return $defaults;}
  function posts_show_id($column_name, $id) {if ($column_name === 'wps_post_id') echo $id;}
  function posts_id_style() {print '<style>#wps_post_id{width:4em}</style>';}
}


/////////////////// узнать все размеры картинок ///////////////////////////////
/**
 * Получает информацию обо всех зарегистрированных размерах картинок.
 * 
 * @global $_wp_additional_image_sizes
 * @uses   get_intermediate_image_sizes()
 * 
 * @param  boolean [$unset_disabled = true] Удалить из списка размеры с 0 высотой и шириной?
 * @return array Данные всех размеров.
 */
function get_image_sizes( $unset_disabled = true ) {
	$wais = & $GLOBALS['_wp_additional_image_sizes'];
	$sizes = array();
	foreach ( get_intermediate_image_sizes() as $_size ) {
		if ( in_array( $_size, array('thumbnail', 'medium', 'medium_large', 'large') ) ) {
			$sizes[ $_size ] = array(
				'width'  => get_option( "{$_size}_size_w" ),
				'height' => get_option( "{$_size}_size_h" ),
				'crop'   => (bool) get_option( "{$_size}_crop" ),
			);
		}
		elseif ( isset( $wais[$_size] ) ) {
			$sizes[ $_size ] = array(
				'width'  => $wais[ $_size ]['width'],
				'height' => $wais[ $_size ]['height'],
				'crop'   => $wais[ $_size ]['crop'],
			);
		}

		// size registered, but has 0 width and height
		if( $unset_disabled && ($sizes[ $_size ]['width'] == 0) && ($sizes[ $_size ]['height'] == 0) )
			unset( $sizes[ $_size ] );
	}
	return $sizes;
}
вызов:
print_r( get_image_sizes() );
/*
Array
(
	[thumbnail] => Array
		(
			[width] => 80
			[height] => 80
			[crop] => 1
		)
//////////////Запрет на вставку ссылок в комментарий////////////////////
add_filter('preprocess_comment', 'href_in_comment');
function href_in_comment($commentdata) {
    if (preg_match("#(<a href=|\[url=|\[link=)?(ftp://|https?://|www)?([\s]?)([^-a-z0-9_@]+)([-a-z0-9/.\s]+\.[a-z]{2,6}[-a-z0-9_/.]*[html|php|cgi]*[\]>]?)+#is", $commentdata['comment_content'])) {
        wp_die('HTML тег ссылки (активная ссылка) в комментариях запрещён. Вернитесь и отредактируйте сообщение.<br /><br /><a href="javascript:history.go(-1);">вернуться назад и отредактировать комментарий</a>');
    }
    return $commentdata;
}

//////////////Добавим фильтр CPT в админку////////////////
function filter_blogs_by_taxonomies( $post_type, $which ) {
    // Apply this only on a specific post type
    if ( 'blog' !== $post_type )
        return;
    // A list of taxonomy slugs to filter by
    $taxonomies = array( 'blogs_post');
    foreach ( $taxonomies as $taxonomy_slug ) {
        // Retrieve taxonomy data
        $taxonomy_obj = get_taxonomy( $taxonomy_slug );
        $taxonomy_name = $taxonomy_obj->labels->name;
        // Retrieve taxonomy terms
        $terms = get_terms( $taxonomy_slug );
        // Display filter HTML
        echo "<select name='{$taxonomy_slug}' id='{$taxonomy_slug}' class='postform'>";
        echo '<option value="">' . sprintf( esc_html__( 'Показать все %s', 'text_domain' ), $taxonomy_name ) . '</option>';
        foreach ( $terms as $term ) {
            printf(
                '<option value="%1$s" %2$s>%3$s (%4$s)</option>',
                $term->slug,
                ( ( isset( $_GET[$taxonomy_slug] ) && ( $_GET[$taxonomy_slug] == $term->slug ) ) ? ' selected="selected"' : '' ),
                $term->name,
                $term->count
            );
        }
        echo '</select>';
    }
}
add_action( 'restrict_manage_posts', 'filter_blogs_by_taxonomies' , 10, 2);

/////////////////////Добавляем кнопку Код темы в админ панель////////////////////////////////
function admin_bar_theme_editor_option() {
    global $wp_admin_bar;
        if ( !is_super_admin() || !is_admin_bar_showing() )
        return;
        $wp_admin_bar->add_menu(
            array( 'id' => 'edit-theme',
            'title' => __('Код темы'),
                        'href' => admin_url( 'theme-editor.php')
        )
        );
    }
add_action( 'admin_bar_menu', 'admin_bar_theme_editor_option', 100 );
//////////////////////////////////////////////////////////////////////////////////
/*
 * $num число, от которого будет зависеть форма слова
 * $form_for_1 первая форма слова, например Товар
 * $form_for_2 вторая форма слова - Товара
 * $form_for_5 третья форма множественного числа слова - Товаров
 */
function true_wordform($num, $form_for_1, $form_for_2, $form_for_5){
    $num = abs($num) % 100; // берем число по модулю и сбрасываем сотни (делим на 100, а остаток присваиваем переменной $num)
    $num_x = $num % 10; // сбрасываем десятки и записываем в новую переменную
    if ($num > 10 && $num < 20) // если число принадлежит отрезку [11;19]
        return $form_for_5;
    if ($num_x > 1 && $num_x < 5) // иначе если число оканчивается на 2,3,4
        return $form_for_2;
    if ($num_x == 1) // иначе если оканчивается на 1
        return $form_for_1;
    return $form_for_5;
}
Использование
 echo $find_prod . ' ' . true_wordform($find_prod, 'товар', 'товара', 'товаров');?>
/////////////////////// номера пунктов меню в админке /////////////////////////
 2 Консоль
4 Разделитель
5 Посты
10 Медиа
15 Ссылки
20 Страницы
25 Комментарии
59 Разделитель
60 Внешний вид
65 Плагины
70 Пользователи
75 Инструменты
80 Настройки
99 Разделитель

////////////////////////////////
/** количество постов выводимых на стр. архивов - и произвольный тип записей **/
function hwl_home_pagesize( $query ) {
// Пример вывода, если это админ-панель или не основной запрос.
if ( is_admin() || ! $query->is_main_query() )
return;

if ( is_home() ) {
// Выводим только 8 постов на главной странице
$query->set( 'posts_per_page', 8 );
return;
}
if ( is_tag() ) {
// Выводим только 8 постов на странице тегов
$query->set( 'posts_per_page', 8 );
return;
}
if ( is_category() ) {
// Выводим только 8 постов на странице категорий
$query->set( 'posts_per_page', 8 );
return;
}
if ( is_search() ) {
// Выводим только 8 постов на странице поиска и т.д.
$query->set( 'posts_per_page', 8 );
return;
}
// пример вывода количества записей архива tv-set
if ( is_post_type_archive( 'tv-set' ) ) {
// Выводим 20 записей если это архив типа записи 'tv-set'
$query->set( 'posts_per_page', 20 );
return;
}
}
add_action('pre_get_posts', 'hwl_home_pagesize', 1 );

///////////////////////////////////////////////////
/**
 *
 * @see http://codex.wordpress.org/Theme_Review#Guidelines
 */
add_filter( 'single_template', 'wisedev_single_template' );

/**
 * Add category considerations to the templates WordPress uses for single posts
 *
 * @global obj $post The default WordPress post object. Used so we have an ID for get_post_type()
 * @param string $template The currently located template from get_single_template()
 * @return string The new locate_template() result
 */
function wisedev_single_template( $template ) {
    global $post;

    $categories = get_the_category();

    if ( ! $categories )
        return $template; // no need to continue if there are no categories

    $post_type = get_post_type( $post->ID );

    $templates = array();

    foreach ( $categories as $category ) {

        $templates[] = "single-{$post_type}-{$category->slug}.php";

        $templates[] = "single-{$post_type}-{$category->term_id}.php";
    }

    // remember the default templates

    $templates[] = "single-{$post_type}.php";

    $templates[] = 'single.php';

    $templates[] = 'index.php';

    /**
     * Let WordPress figure out if the templates exist or not.
     *
     * @see http://codex.wordpress.org/Function_Reference/locate_template
     */
    return locate_template( $templates );
}
///////////////////////////////////////////////////////////////////////////
/**
 * Add an automatic default custom taxonomy for custom post type.
 * If no story (taxonomy) is set, the comic post will be sorted as “draft” and won’t return an offset error.
 *
 */
function set_default_object_terms( $post_id, $post ) {
    if ( 'publish' === $post->post_status && $post->post_type === 'comic' ) {
        $defaults = array(
            'story' => array( 'draft' )
                );
            $taxonomies = get_object_taxonomies( $post->post_type );
            foreach ( (array) $taxonomies as $taxonomy ) {
                $terms = wp_get_post_terms( $post_id, $taxonomy );
                if ( empty( $terms ) && array_key_exists( $taxonomy, $defaults ) ) {
                    wp_set_object_terms( $post_id, $defaults[$taxonomy], $taxonomy );
                }
            }
        }
}
add_action( 'save_post', 'set_default_object_terms', 0, 2 );