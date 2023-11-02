//insert

 SqlConnection cn = new SqlConnection(@"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\AdminDatabase.mdf;Integrated Security=True");
        SqlCommand cmd = new SqlCommand();
        string qry;
        string fname;
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void Btn_theme_Click(object sender, EventArgs e)
        {


            if (FileUpload1.HasFile)
            {
                if (FileUpload1.PostedFile.ContentType== "image/jpeg")
                {
                    fname = (FileUpload1.FileName);
                    FileUpload1.SaveAs(Server.MapPath("~/Themeparkridepic/" + fname));
                    cn.Open();
                    qry = "insert into Themepark values('" + ddlridecategory.Text + "','" + ddl1.Text + "','" + txttname.Text + "','" + FileUpload1.FileName + "','" + txtdescription.Text + "','" + DropDownList3.Text + "')";
                    cmd = new SqlCommand(qry, cn);
                    cmd.ExecuteNonQuery();
                    cn.Close();
                    Response.Redirect("Themepark.aspx");
                }
                else
                {
                    Label1.Text = "please select image file...";
                }
            }



        }

//display

<asp:SqlDataSource ID="SqlDataSource3" runat="server" ConnectionString='<%$ ConnectionStrings:ConnectionString %>' SelectCommand="SELECT Themepark.themerideid, Themepark.Ridecatid, Themepark.themercid, Themepark.ridename, Themepark.rideimg, Themepark.ridediscription, Themepark.status, Ride_category.Ridecatid AS Expr1, Ride_category.ddlridecategory, themerc.pic, themerc.themercname, themerc.themercid AS Expr2 FROM Themepark INNER JOIN themerc ON Themepark.themercid = themerc.themercid INNER JOIN Ride_category ON Themepark.Ridecatid = Ride_category.Ridecatid"></asp:SqlDataSource>
                                <asp:Repeater ID="Repeater3" runat="server" DataSourceID="SqlDataSource3">
                                    <HeaderTemplate>

                                                <table id="html5-extension" class="table table-hover non-hover" style="width: 100%">
                                                    <h4>Themepark Table</h4>
                                                    <thead>
                                                        <tr>
                                                            <th>Theme Id</th>
                                                            <th>Ride Category Name</th>
                                                            <th>Theme Category Name</th>
                                                            <th>Theme Ride Name</th>
                                                            <th>Ride Pic</th>
                                                            <th>Descriptions</th>
                                                            <th>status</th>

                                                            <th class="dt-no-sorting">Action</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                    </HeaderTemplate>
                                    <ItemTemplate>
                                        <tr>
                                            <td class="checkbox-column text-center sorting_1"><%#Eval("themerideid") %></td>
                                            <td><%#Eval("ddlridecategory") %></td>
                                            <td><%#Eval("themercname") %></td>
                                            <td><%#Eval("ridename") %></td>
                                            <td>
                                                <img src='../Themeparkridepic/<%#Eval("rideimg") %>' height="100" width="100" />
                                            </td>
                                            <td><%#Eval("ridediscription") %></td>
                                            <td><%#Eval("status") %></td>


                                            <td>
                                                <div class="btn-group">
                                                    <button type="button" class="btn btn-dark btn-sm">Action</button>
                                                    <button type="button" class="btn btn-dark btn-sm dropdown-toggle dropdown-toggle-split" id="dropdownMenuReference1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" data-reference="parent">
                                                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-down">
                                                            <polyline points="6 9 12 15 18 9"></polyline></svg>
                                                    </button>


                                                    <div class="dropdown-menu" aria-labelledby="dropdownMenuReference1">
                                                        <a class="dropdown-item" href="UpdateTheme.aspx?themerideid=<%#Eval("themerideid") %>">Update</a>
                                                        <a class="dropdown-item" href="DeleteTheme.aspx?themerideid=<%#Eval("themerideid") %>">Delete</a>

                                                    </div>
                                                </div>
                                            </td>
                                        </tr>
                                    </ItemTemplate>
                                    <FooterTemplate>
                                        </tbody>
                            </table>
                       
                                    </FooterTemplate>
                                </asp:Repeater>
//update
public partial class WebForm2 : System.Web.UI.Page
    {
        SqlConnection cn = new SqlConnection(@"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\AdminDatabase.mdf;Integrated Security=True");
        SqlCommand cmd = new SqlCommand();
        string qry;
        string fname;
        SqlDataReader dr;
        int aid;
        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {
                ViewState["themerideid"] = Convert.ToInt32(Request.QueryString.Get("themerideid"));
                aid = Convert.ToInt32(ViewState["themerideid"].ToString());
                cn.Open();
                qry = "select * from Themepark where themerideid= " + aid;
                cmd = new SqlCommand(qry, cn);
                dr = cmd.ExecuteReader();
                if (dr.HasRows)
                {
                    dr.Read();
                    txttheme.Text = dr["ridename"].ToString();
                    txtdis.Text = dr["ridediscription"].ToString();
                    Image1.ImageUrl = "../Themeparkridepic/" + dr["rideimg"].ToString();
                    DropDownList3.Text = dr["status"].ToString();
                }
                cn.Close();
            }
        }

        protected void btnsub_Click(object sender, EventArgs e)
        {
            if (FileUpload1.HasFile)
            {
                if (FileUpload1.PostedFile.ContentType == "image/jpeg")
                {
                    fname = (FileUpload1.FileName);
                    FileUpload1.SaveAs(Server.MapPath("~/Themeparkridepic/" + fname));
                    aid = Convert.ToInt32(ViewState["themerideid"].ToString());
                    cn.Open();
                    qry = "update Themepark set ridename='" + txttheme.Text + "', rideimg='" + FileUpload1.FileName + "', ridediscription='" + txtdis.Text + "', status='" + DropDownList3.Text + "' where themerideid=" + aid;
                    cmd = new SqlCommand(qry, cn);
                    cmd.ExecuteNonQuery();
                    cn.Close();
                    Response.Redirect("Themepark.aspx");
                }
            }


        }
    }

//delete
 SqlConnection cn = new SqlConnection(@"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\AdminDatabase.mdf;Integrated Security=True");
        SqlCommand cmd = new SqlCommand();
        string qry;
        int aid;
        protected void Page_Load(object sender, EventArgs e)
        {
            ViewState["themerideid"] = Convert.ToInt32(Request.QueryString.Get("themerideid"));
            aid = Convert.ToInt32(Request.QueryString.Get("themerideid"));/* Convert.ToInt32(ViewState["Cid"].ToString());*/
            cn.Open();
            qry = "delete from Themepark where themerideid=" + aid;
            cmd = new SqlCommand(qry, cn);
            cmd.ExecuteNonQuery();
            cn.Close();
            Response.Redirect("Themepark.aspx");
        }
