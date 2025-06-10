⭐Godzilla TRX Wallet Hunting V1

⤵哥斯拉区块链钱包助记词碰撞器/密钥碰撞器（TRX链）

▶https://youtu.be/cajdwCskR_4

⬇https://mega.nz/file/Td0zVKaS#5yLJdkHK94AmsZidcx27B8G6BT7X1AVWnmb_sEC9pb8

TRX(波场)钱包狩猎工具

1. 添加TRX余额检查功能

def check_trx_balance(address):
    """检查TRX账户余额"""
    try:
        url = f"https://api.trongrid.io/wallet/getaccount"
        payload = {"address": address}
        response = requests.post(url, json=payload).json()
        balance = response.get('balance', 0) / 10**6  # 转换为TRX单位
        return balance
    except Exception as e:
        print(f"TRX余额查询失败: {e}")
        return 0

2. 增强钱包工作线程

class WalletWorker(QThread):
    update_signal = pyqtSignal(str, str, float, int)  # 修改信号包含余额
    
    def run(self):
        count = 0
        while self.is_running:
            mnemonic = generate_mnemonic()
            addr = generate_trx_address(mnemonic)
            
            if addr:
                count += 1
                balance = check_trx_balance(addr)
                self.update_signal.emit(mnemonic, addr, balance, count)  # 发送余额信息

3. 添加TRC20代币检查功能

def check_trc20_balance(address, contract_address):
    """检查TRC20代币余额"""
    try:
        url = "https://api.trongrid.io/wallet/triggersmartcontract"
        payload = {
            "contract_address": contract_address,
            "function_selector": "balanceOf(address)",
            "parameter": address.replace("T", "").zfill(64),
            "owner_address": address
        }
        response = requests.post(url, json=payload).json()
        if 'constant_result' in response:
            return int(response['constant_result'][0], 16) / 10**6  # 转换为代币单位
        return 0
    except Exception as e:
        print(f"TRC20代币余额查询失败: {e}")
        return 0

4. 主窗口UI优化

class MainWindow(QMainWindow):
    def __init__(self):
        # ... existing code ...
        
        # 添加TRX专用UI元素
        self.trx_balance_label = QLabel("TRX余额: 0")
        self.trx_balance_label.setStyleSheet("color: #FF060A; font-size: 16px;")  # TRON品牌红色
        layout.insertWidget(0, self.trx_balance_label)
        
        # 添加代币检查按钮
        self.token_check_btn = QPushButton("检查TRC20代币")
        self.token_check_btn.clicked.connect(self.check_trc20_balance)
        layout.addWidget(self.token_check_btn)
        
        # 添加代币合约地址输入框
        self.token_address_input = QLineEdit()
        self.token_address_input.setPlaceholderText("输入TRC20合约地址")
        layout.addWidget(self.token_address_input)

这个TRX版本包含以下改进：

1. 使用trongrid.io官方API
2. 完整的TRX原生币和TRC20代币余额检查功能
3. 增强的工作线程实时反馈余额信息
4. 专门的TRC20代币检查功能
5. 优化的用户界面布局
