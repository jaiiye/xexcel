#xexcel<br />
excel import/export util，Be based on jxl implement.<br />
More explanation: http://www.yaxin.tk<br />
import by maven<br />
&lt;dependency&gt;<br />
&lt;groupId&gt;tk.yaxin&lt;/groupId&gt;<br />
&lt;artifactId&gt;xexcel&lt;/artifactId&gt;<br />
&lt;version&gt;0.0.1-Releases&lt;/version&gt;<br />
&lt;/dependency&gt;

#use example:<br />
##pojo:<br />
public class User {<br />
	@ExcelField(lableName="name")<br />
	private String name;<br />
	@ExcelField(lableName="sex",covertClass=SexConvert.class)<br />
	private String sex;<br />
	public String getName() {<br />
		return name;<br />
	}<br />
	public void setName(String name) {<br />
		this.name = name;<br />
	}<br />
	public String getSex() {<br />
		return sex;<br />
	}<br />
	public void setSex(String sex) {<br />
		this.sex = sex;<br />
	}<br />
}<br />
##convert class:<br />
public class SexConvert implements ExcelConvert{<br />
	public Object convertToObj(String obj) {<br />
		if("man".equals(obj))<br />
			return 0;<br />
		else<br />
			return 1;<br />
	}<br />
	public Object convertToExcel(String obj) {<br />
		if("0".equals(obj))<br />
			return "man";<br />
		else<br />
			return "woman";<br />
	}<br />
}<br />
##main class:<br />
public class Test {<br />
	UserDAO userDAO;<br />
	public void export(HttpServletResponse response){<br />
		response.setContentType("application/x-msdownload;");  <br />
        response.setHeader("Content-disposition", "attachment; filename="  <br />
                 +  java.net.URLEncoder.encode("user.xls", "UTF-8"));<br />
		List<User> list=userDao.getUser();<br />
		ByteArrayOutputStream os = new ByteArrayOutputStream();<br />
		ExcelUtil.export(User.class, list, os);<br />
		byte[] files = os.toByteArray();<br />
		response.setHeader("Content-Length", String.valueOf(files.length)); <br />
		response.getOutputStream().write(files);<br />
		os.close();<br />
	}<br />
}
