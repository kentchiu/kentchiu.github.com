---
author: Kent Chiu
published: true
layout: post
title: "顺风速运货到付款插件"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



安裝
----

- 把文件夹上传到网站的根目录(覆盖) -
到后台配送方式－选择货到付款－安装－设置区域和货到付款的费率

OK，只需两步即可搞定，此插件是基于顺风速运货到付款

目錄結構
--------


```
includes
|- modules
    |- shopping
       |- express_cod.php

languages
|- zh-cn
   |- shopping
      |- express_cod.php

```

主程式
------



```
    <?php
     
    /**
     * ECSHOP 顺丰速运货到付款 配送方式插件
     * ============================================================================
     * 版权所有 (C) 2005-2008 康盛创想（北京）科技有限公司，并保留所有权利。
     * 网站地址: http://www.ecshop.com；http://www.comsenz.com
     * ----------------------------------------------------------------------------
     * 这不是一个自由软件！您只能在不用于商业目的的前提下对程序代码进行修改和
     * 使用；不允许对程序代码以任何形式任何目的的再发布。
     * ============================================================================
     * $Author: testyang $
     * $Id: express_cod.php 14481 2008-04-18 11:23:01Z testyang $
     */
     
    if (!defined('IN_ECS'))
    {
        die('Hacking attempt');
    }
     
    $shipping_lang = ROOT_PATH.'languages/' .$GLOBALS['_CFG']['lang']. '/shipping/express_cod.php';
     
    if (file_exists($shipping_lang))
    {
        global $_LANG;
        include_once($shipping_lang);
    }
     
    /* 模块的基本信息 */
    if (isset($set_modules) && $set_modules == TRUE)
    {
        $i = (isset($modules)) ? count($modules) : 0;
     
        /* 配送方式插件的代码必须和文件名保持一致 */
        $modules[$i]['code']    = basename(__FILE__, '.php');
     
        $modules[$i]['version'] = '1.0.0';
     
        /* 配送方式的描述 */
        $modules[$i]['desc']    = 'express_desc';
     
        /* 配送方式是否支持货到付款 */
        $modules[$i]['cod']     = TRUE;
     
        /* 插件的作者 */
        $modules[$i]['author']  = '好车饰';
     
        /* 插件作者的官方网站 */
        $modules[$i]['website'] = 'http://www.carhao.com';
     
        /* 配送接口需要的参数 */
        $modules[$i]['configure'] = array(
                                        array('name' => 'basic_fee',    'value'=>15), /* 1000克以内的价格           */
                                        array('name' => 'step_fee',     'value'=>2),  /* 续重每1000克增加的价格 */
                                    );
     
        return;
    }
     
    /**
     * 顺丰速运费用计算方式: 起点到终点 * 重量(kg)
     * ====================================================================================
     * -浙江，上海，江苏地区为15元/公斤，续重(2元/公斤)
     * -续重每500克或其零数 (具体请上顺丰速运网站查询:http://www.sf-express.com/sfwebapp/price.jsp 客服电话 4008111111)
     *
     * -------------------------------------------------------------------------------------
     */
     
    class express_cod
    {
        /*------------------------------------------------------ */
        //-- PUBLIC ATTRIBUTEs
        /*------------------------------------------------------ */
     
        /**
         * 配置信息参数
         */
        var $configure;
     
        /*------------------------------------------------------ */
        //-- PUBLIC METHODs
        /*------------------------------------------------------ */
     
        /**
         * 构造函数
         *
         * @param: $configure[array]    配送方式的参数的数组
         *
         * @return null
         */
        function express_cod($cfg=array())
        {
            foreach ($cfg AS $key=>$val)
            {
                $this->configure[$val['name']] = $val['value'];
            }
     
        }
     
        /**
         * 计算订单的配送费用的函数
         *
         * @param   float   $goods_weight   商品重量
         * @param   float   $goods_amount   商品金额
         * @return  decimal
         */
        function calculate($goods_weight, $goods_amount)
        {
            if ($this->configure['free_money'] > 0 && $goods_amount >= $this->configure['free_money'])
            {
                return 0;
            }
            else
            {
                @$fee = $this->configure['basic_fee'];
     
                if ($goods_weight > 1)
                {
                    $fee += (ceil(($goods_weight - 1))) * $this->configure['step_fee'];
                }
               // $_SESSION['cart_weight'] = $goods_weight;
                return $fee;
            }
        }
     
        /**
         * 查询快递状态
         *
         * @access  public
         * @return  string  查询窗口的链接地址
         */
        function query($invoice_sn)
        {
            $form_str = '<a href="http://www.sf-express.com/sfwebapp/track.jsp?tracklist='.$invoice_sn.'" target="_blank">' .$invoice_sn. '</a>';
     
            return $form_str;
        }
    }
     
    ?>

```

語系程式
--------



```
    <?php
     
    /**
     * ECSHOP 顺丰速运货到付款插件的语言文件
     * ============================================================================
     * 版权所有 (C) 2005-2008 康盛创想（北京）科技有限公司，并保留所有权利。
     * 网站地址: http://www.ecshop.com；http://www.comsenz.com
     * ----------------------------------------------------------------------------
     * 这不是一个自由软件！您只能在不用于商业目的的前提下对程序代码进行修改和
     * 使用；不允许对程序代码以任何形式任何目的的再发布。
     * ============================================================================
     * $Author: testyang $
     * $Id: express_cod.php 14481 2008-04-18 11:23:01Z testyang $
    */
    global $_LANG;
     
    $_LANG['express_cod']            = '货到付款';
    $_LANG['express_desc']           = '江、浙、沪地区首重15元/KG，续重2元/KG，其余城市首重20元/KG';
    $_LANG['basic_fee']              = '1000克以内费用';
    $_LANG['step_fee']               = '续重每1000克或其零数的费用';
     
    ?>

```

