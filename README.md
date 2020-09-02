データ漏れ

SELECT  [ZB_RD_COST_PROJ].[COST_PROJ_CODE] AS [COST_PROJ_CODE],  [ZB_RD_COST_PROJ].[COST_PROJ_SBNO] AS [COST_PROJ_SBNO],  [ZB_RD_COST_PROJ].[COST_PROJ_NAME] AS [COST_PROJ_NAME] FROM 

[R_1_1_0_CM].[dbo].[APPROVAL_APPL] AS [APPL]  LEFT OUTER JOIN 
[R_1_1_0_SC].[dbo].[QUOTE]  

ON [APPL].[SLIP_NO] = CONCAT([QUOTE].[QUOTE_NO],[QUOTE].[QUOTE_SBNO])  
LEFT OUTER JOIN 
[R_1_1_0_SC].[dbo].[ZL_PROJ_FILE_CHECK_LIST_DTIL] AS [CHECKLIST] 
  
ON [QUOTE].[PROJ_CODE] = [CHECKLIST].[PROJ_CODE] AND [QUOTE].[PROJ_SBNO] = [CHECKLIST].[PROJ_SBNO]  LEFT OUTER JOIN 

[R_1_1_0_RD].[dbo].[ZB_RD_COST_PROJ] 

ON [CHECKLIST].[PROJ_CODE] = [ZB_RD_COST_PROJ].[COST_PROJ_CODE] AND [CHECKLIST].[PROJ_SBNO] = [ZB_RD_COST_PROJ].[COST_PROJ_SBNO]  
WHERE  [APPL].[APPROVAL_ROUTE_TYPE] = 'SD2'  AND [APPL].[SLIP_NO] = 'Q00000002400' 


--06101-01

UPDATE [R_1_1_0_SC].[dbo].[QUOTE] set PROJ_CODE='06101',PROJ_SBNO='01'  WHERE   QUOTE_NO='Q000000024'


- - -
追加一个方法
SelectApprovalEmpCode
- - -
获取mark
private bool mark = false;
mark = SelectApprovalEmpCode(CommonData, approvalRouteType, slipNo, this.UserCode);
        public static bool SelectApprovalEmpCode(CommonData commonData, string approvalRouteType, string slipNo, string userCode)
        {
            bool approvelMark = BL_CM_WF_03_S03.SelectApprovalEmpCode(commonData, approvalRouteType, slipNo, userCode);
            return approvelMark;
        }
- - -
利用mark
                   EncodeLabel AttachmentUserCode = (EncodeLabel)e.Item.FindControl("AttachmentUserCode");
                    // 管理番号 K25667 From
                    //					if (ackType != "A")
                    //						DeleteButton.Enabled = false;
                    // 画面が参照モードの時、削除ボタンを非活性とする
                    DeleteButton.Enabled = base.Mode != PageMode.Reference;
                    // 管理番号 K25667 To
                    String userCode = AttachmentUserCode.Text.ToString().Trim();
                    AttachmentUserCode.Visible = false;

                    if (mark)
                    {
                        TypeRadio.Enabled = true;
                        FileLabel.Enabled = true;
                        FileLink.Enabled = true;

                        if (userCode == this.UserCode)
                        {
                            DeleteButton.Enabled = true;
                        }
                    }

- - -
页面增加隐藏
<cc1:EncodeLabel id="AttachmentUserCode" Runat="server" Text='<%# DataBinder.Eval(Container.DataItem, "[UPDATE_USER_CODE]") %>'>
<cc1:EncodeLabel id="AttachmentUserCode" Runat="server" Text='<%# DataBinder.Eval(Container.DataItem, "[UPDATE_USER_CODE]") %>'>					
												</cc1:EncodeLabel>
- - -
增加表格查询字段UPDATE_USER_CODE 绑定页面
public static DataTable Select(CommonData commonData, IF_CM_WF_N3_S14_SearchCondition searchCondition, SqlConnection cn)
sb.Append(",[APPROVAL].[UPDATE_USER_CODE]");
- - -
登录
public static void Insert(
		CommonData commonData, IF_CM_WF_N3_S14_SearchCondition searchCondition, ZL_IF_CM_WF_RowData rowData, SqlConnection cn ,SqlTransaction tx,string userCode)
 sb.Append(",[UPDATE_USER_CODE]");
 sb.Append(",@UpdateUserCode");
 cm.Parameters.Add("@ProjectCode", SqlDbType.NVarChar, 10).Value = ConvertDBData.ToNVarChar(searchCondition.ProjectCode);
- - -
调用部分增加参数
public static ZL_IF_CM_WF_AttachmentFiles Insert(CommonData commonData, IF_CM_WF_N3_S14_SearchCondition searchCondition, ZL_IF_CM_WF_RowData rowData,string userCode)
Insert(commonData, searchCondition, rowData, cn, tx,userCode);


private bool insertRecord(DataRow newRow)


A000000045

- - - 
GetDocumentName
