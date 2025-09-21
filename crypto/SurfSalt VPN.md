## SurfSalt VPN
hackmd 連結 : [SurfSalt VPN](https://hackmd.io/@yeyeye618/SkAd2O6iee)
![image](https://hackmd.io/_uploads/SJSf6daixe.png)
給了一整段的加密文章
```
Alc Getrptrezxv Uoox: Alc Mxw oq moi Tuxwnècx Jmntvj

Fzk jilfljipl, alc mil oq vycnffyrlioc umj s czgzxyzk taemsi zqkoepg jslovslxxux yzu jegxseruff. Stgji rtv wrl hm Nsxzms Ntlwyd, jamael wsnjlienammz tapsxyw badanlmlh rtv oocek sd evurpm tiqeryed. Bu xfqjw sjlaike, r deemlv umj uoylpwrqellj kltjmtwd mr hrmfywr, l lpqnxv lrtvr xfmk urptaib m wsclwl sd evuucbac. Fanwvpk, ic rtv 9lh nxuxsdp, trtesmyzk eiywz ml fyw Acti amdcv hlw wiprvutpw mvcclwnnr hryxpkid, t tirtfv oq vvylfzfg wxaxcd fucfkyilovk tz lfwrqdsttvhpjk sjeld alcev uooxz. Jmd ymnoklhq aw qelkz, gmpv eavxyw aaldd zgsc advstp ibdxxvk tsta, agfy wnznnl rudw ayw prrqcdenm, jsbqsjeldlvq ofmlo buzydzsbwr zsjhv. Kenklgw irk fctnmjq rfd exttmdrjy.

Qxlpgzx divx fssd jwccxaw ydvf't dtmi? Uujziyz msp me mnnkhgimsde dhsyruff iy rvyp pralj epjc? Uelrzwbggzx "KucyZejf MHN™" – eal vchfduebvrydp feh twt rtrl udxz ebhrfcpw… dijx, egt bnpxc Hzyeyèkl, fsf zl’s dmppj biwter nsmp! JmrqLhpr HGF id lbtcd jwcfkl, pgwv qof'kl iloiqpebuk gf wgr jhbvqqcx: id1tiGRR{JmrqLhpr_HGF_id_LbGf_4_SiwAe_WleJ}. Pfonwhhh rausy qhy 15% sdr pguc ypvqf pwac hm yjfzeaex kmeuksl akpzyop! (Gfqxy rmf msltw vr yokmaw fppgfrjy-rkhhc qeurjiammz.) Egw, mtjo ra fmr cxnyjmidy dvoibgcwd sbzxmdp!

Lhtl hpj nvyay mv gfmeye tg 1553, dmrt Xaogtu Fyfkaset Iijxrko'd wlwadzhtthu sd m eww, cxcsjgkaoytyc qkjlex, ehxcd daslmavgnlleo mv elp esmpw hjrqi lhp Yyiloy viaevqyf Sdatll hc Hzyeyèkl. Xfq Magpgèyi augzec phw y fime atyebuxe ssbmx. Gzjlelw vj sezfg zgl wrmkac nbwlcd rdpstiir, uk wmaevccp r cejpvvb ff khtya fcfnwey 26 wpjdqiwne ivwqusde lewlynvls eaysssygue moi kqjkarx. H pcfkwr wbri 'c' yzyhe ul iloiqpexk eq 'X' zf oyx prqfrfcp tuh 'U' ue lhp glbr, qwxenmpzcxp xllmailuey tsx zxyfzkttvhp ddviupgjc nmklecgz xfmk uooxivcmbwrd klpgqu mpzg.

Alc Hzyeyèkl ggbywr htz wm bfoecybp rtrl ie phw bqveeo iyeafzuawef ylniwavtipc. Rfj tskli aqelucblw, gf vsryxk xfq wgrxbkezxv finduekq "cw csbmjpmxw iywéjlgrwjamel"—xfq zfdpvptfqisbwx jmntvj. Iel jskbcwxtmf tpamadpw hr szgjenxkilfvv lpolp mr jwcfkpxw rfj mtepxydp, viaevqyfzu, ayw wipeffaw vvvpqjhoywlraq. Kzid vycnffyrlioma ekjoyzosjp iwmlbuib evuucx brruc 1854, ohpg Jlydcws Mtifysv, s vtlpslmiq Eyzsmqt dstsxterutaay, ypryxcq dpolpmbvv a drzxcyrlin flxfau lo nkhgi uk. Lhp Opkczèiw ctioip yrjkpw h rci vhona pr adphtzzyentp, anekvhsozfg l vvqnxvpier alyf wmnottilfrdlj tsxcdvv tsx iejmeue zy wsuqi teeplil fygsp pos fuuw spvyire rfd eavwc iyg spxr xfqd.
```

由於先前我嘗試用 cyberchef 看 entropy 相當的高 
![image](https://hackmd.io/_uploads/BkqZkKpixx.png)
所以我懷疑是多表代換的加密 最常見的是 維吉尼亞加密
順著想法猜

可以看到 第一句話 看起來像書名
```
Alc Getrptrezxv Uoox: Alc Mxw oq moi Tuxwnècx Jmntvj
```
出現了兩個 Alc 
這兩個位置出現的 我大膽假設是 The
兩段文字中間相差 18 個字
所以密碼長度應該是 18 的 因數
2 3 6 9 18 (1 不可能)
A 和 c 的差距只有 2 但是
T 和 e 差距不是 2
所以 密碼長度不會為 2

而中間有個看起來應該是 flag 的密
```
id1tiGRR{JmrqLhpr_HGF_id_LbGf_4_SiwAe_WleJ}
```
可以看到 prefix is1abCTF
如果 密碼長度是 3 的話
i 和 b 是不可能加密出 i 和 i 的
如果 密碼長度是 6 的話
i 和 F 是不可能加密出 i 和 R 的
所以目前可以嘗試的就是
密碼長度 9 或 18 的 key

維吉尼亞可以看做是 固定位置的 rot
要能夠 把 Alc 變成 The
那麼密碼的前 3 位應該是 hey

![image](https://hackmd.io/_uploads/ryp0VKTjee.png)
可以用億點點的嘗試 來先猜 9 位數的密碼
目前可以知道的就是
heym??alt

到這裡就可以搬出 is1ab 的密碼學大佬
![image](https://hackmd.io/_uploads/BkyLSKTjle.png)

直接猜 heymrsalt
![image](https://hackmd.io/_uploads/ryIYBtpjlg.png)

完美
flag : `is1abCTF{SurfSalt_VPN_is_SuCh_4_GreAt_DeaL}`