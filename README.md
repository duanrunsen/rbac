<h3>查询用户test1可以查看的页面（Sys_menu）：</h3>
<h4>伪代码：</h4>
1.根据user中用户text1的userID，找出userrole中该用户的roleID，可在role中得到该用户的角色名称。
2.将cf_privilege表与sys_menu表左连接，遍历cf_privilege。
3.P.PrivilegeAccess='Sys_Button' 、PrivilegeOperation='Permit' 、M.MenuName='订单' 、sys_Menu.MenuNo=sys_Menu.MenuNo且if PrivilegeMaster字段，如果是CF_User,则判断PrivilegeMasterKey是否等于在第一步中查找的UserID,如果是，并入查询结果集；如果PrivilegeMaster字段是CF_Role,则判断PrivilegeMasterKey是否等于在第二步中查找的RoleID,如果是，并入查询结果集。
<h4>查询代码：</h4> 
select DISTINCT M.MenuID,M.MenuNo,M.MenuName 
from cf_privilege P left join sys_menu M on P.PrivilegeAccessKey=M.MenuID,cf_user U 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Menu' and PrivilegeOperation='Permit';
<img src="">
<h3>查询用户test1可以对订单(order)页面中的操作权限(sys_button):</h3>
<h4>伪代码：</h4>
1.根据user中用户text1的userID，找出userrole中该用户的roleID。
2.将cf_privilege与sys_menu左连接。
3.在cf_privilege中遍历，P.PrivilegeAccess='Sys_Button' 、PrivilegeOperation='Permit' 、M.MenuName='订单' 、sys_Menu.MenuNo=sys_Menu.MenuNo且if PrivilegeMaster字段，如果是CF_User,则判断PrivilegeMasterKey是否等于在第一步中查找的UserID,如果是，并入查询结果集；如果PrivilegeMaster字段是CF_Role,则判断PrivilegeMasterKey是否等于在第二步中查找的RoleID,如果是，并入查询结果集。
<h4>查询代码：</h4>
select DISTINCT B.BtnName,B.BtnID 
from cf_privilege P left join sys_button B on P.PrivilegeAccessKey=B.BtnID,cf_user U,sys_menu M 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Button' and PrivilegeOperation='Permit' AND M.MenuName='订单' and B.MenuNo=M.MenuNo;
<img src="">
