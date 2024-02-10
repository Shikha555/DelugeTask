# DelugeTask
task 1
//New Lead Creation in Zoho CRM
orgId = "60016104929";
getDetails = invokeurl
[
	url :"https://desk.zoho.in/api/v1/tickets/" + TicketID
	type :GET
	connection:"zohodeskallnew"
];
phone = getDetails.getJSON("phone");
//info "phone===>"+phone;
Email = getDetails.getJSON("email");
//info "\n Email===>"+Email;
mp = Map();
mp.put("Mobile",phone);
mp.put("Email",Email);
new_Lead = zoho.crm.createRecord("Leads",mp,{"trigger":{"workflow"}});
info new_Lead;

//follow up notification

orgId = "60016104929";
getDetails = invokeurl
[
	url :"https://desk.zoho.in/api/v1/tickets/" + TicketID
	type :GET
	connection:"zohodeskallnew"
];
cf = getDetails.getjson("cf");
followUpTime = cf.getJSON("cf_follow_up_date_time");
reminder_time = created_time.addDays(2).toTime();
info reminder_time;
mp = Map();
mp.put("Remind_At",{"ALARM":"FREQ=NONE;ACTION=POPUp;TRIGGER=DATE-TIME:" + reminder_time.toString("yyyy-MM-dd'T'HH:mm:ss'+05:30'")});
mp.put("Pop_up",true);
update_record = zoho.desk.updateRecord("Tasks",taskid,mp);
