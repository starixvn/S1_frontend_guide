# VPN

> VPN 連線有助於提供更安全的公司網路和網際網路連線和存取

## 目錄

- [VPN](#vpn)
  - [目錄](#目錄)
  - [ailab 連線說明](#ailab-連線說明)
  - [Windows](#windows)
  - [Mac OS](#mac-os)

## ailab 連線說明

- VPN 連線名稱輸入可識別的名稱。例如：`ailab@l2tp`
- VPN 伺服器位址輸入：`218.32.244.12`
- VPN 類型選擇：`L2TP/IPSec`
- VPN 預先共用金鑰：`L2TP@ailab`
- VPN 帳號密碼：填寫公司提供的 nas 帳號密碼，通常會寄信到信箱

## Windows

> 設定完後需要再點開 **Windows 新增 L2TP 機碼 .bat** 檔，完成設定。

1. 選取開始 按鈕，然後選取設定> 網路& VPN >新增>VPN 連接。

2. 在 [新增 VPN 連線] 中，執行下列動作：

   - 針對 [VPN 提供者]，選擇 [Windows (內建)]。
   - 在連接 名稱方塊 中，輸入一個您能夠識別 (的名稱，例如，我的個人 VPN) 。 這是您在連線時要尋找的 VPN 連線名稱。
   - 在 _伺服器名稱或位址_ 方塊中，輸入 VPN 伺服器的位址。
   - 針對 [VPN 類型]，選擇您要建立的 VPN 連線類型。 您必須知道公司或 VPN 服務使用的 VPN 連線類型。
   - 針對 登錄資訊類型，選擇您 (或認證) 類型。 這可以是使用者名稱和密碼、單次密碼、憑證，或者如果您連到 VPN 工作，也可以使用智慧卡。 在個別的方塊中輸入使用者名稱與密碼 (如果必填)。

3. 選取 [儲存]。

> <https://support.microsoft.com/zh-hk/windows/%E9%80%A3%E7%B7%9A%E4%B8%AD%E7%9A%84-vpn-windows-3d29aeb1-f497-f6b7-7633-115722c1009c#WindowsVersion=Windows_10>

## Mac OS

1. 在 Mac 上，選擇「蘋果」選單 >「系統偏好設定 ⋯」，然後按一下「網路」。

2. 按一下左側列表的「加入」按鈕 ，按一下「介面」彈出式選單，然後選擇 VPN。

3. 按一下「VPN 類型」彈出式選單，然後依據您要連線的網路，來選擇您要設定的 VPN 連線類型。為 VPN 服務命名，然後按一下「製作」。

   - 由「Internet 網路服務供應商」所使用的 L2TP 是「點對點通道協定」（Point-to-Point Tunneling Protocol）的延伸協定，能讓「Internet 網路服務供應商」透過 Internet 來啟用 VPN。
   - IPSec（Internet Protocol Security）是一種安全通訊協定。
   - IKEv2 是一種會在 IPSec 中設定安全性關聯的通訊協定。

4. 輸入 VPN 連線的伺服器位址和帳號名稱。

5. 按一下「認證設定 ⋯」，然後輸入從網路管理者收到的資訊。

6. 若由網路管理員指定，請按一下「進階」來輸入其他資訊，例如連線時段選項、TCP/IP 設定、DNS 伺服器和代理伺服器。

可輸入的其他資訊取決於您設定的 VPN 連線類型。

按一下「套用」，然後按一下「好」。

> <https://support.apple.com/zh-tw/guide/mac-help/mchlp2963/mac>
