     <!-- Main content -->
	<link rel="stylesheet" href="plug/jquery-ui.css">
	<script src="plug/jquery-3.6.0.min.js"></script>
	<script src="plug/jquery-ui.js"></script>
	<script type="text/javascript" src="http://code.jquery.com/qunit/qunit-1.11.0.js"></script>	  
	<script type="text/javascript" src="./sinon-1.10.3.js"></script>
	<script type="text/javascript" src="./sinon-qunit-1.0.0.js"></script>
    <script type="text/javascript" src="../src/jquery.mask.js"></script>
	<script type="text/javascript" src="jquery.mask.test.js"></script>
	<section class="content">
		<div class="container-fluid">
		<div class="row">
		<div class="col-12">         
            <!-- /.card -->
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Data Nomor Surat Permohonan Kredit Konsumer</h3>
              </div>
              <!-- /.card-header -->
              <div class="card-body">
			   <button type="button" class="btn btn-info" data-toggle="modal" data-target="#modal-lg">
                  Tambah Data
                </button>
				<br></br>
                <table id="example1" class="table table-bordered table-striped">
                  <thead>
                  <tr>
                    <th>No</th>
					<th>Kode Surat</th>
					<th>Kode Cabang/Capem</th>
					<th>Nama Cabang/Capem</th>
					 <th>Perihal</th>
					<th>Asal Surat</th>
					<th>Tanggal Surat</th>
					<th>Action</th>
					</tr>
                  </thead>
                  <tbody>
				  <?php
				include_once "../library/inc.connection.php";
				include_once "../library/inc.library.php";
				$mySql 	= "SELECT suratmasuk.*, cabang.kdcabang, cabang.namacab FROM suratmasuk
				LEFT JOIN cabang ON suratmasuk.kdcabang=cabang.kdcabang 
				ORDER BY suratmasuk.kdsurat ASC";
			    $myQry = mysqli_query($koneksidb, $mySql) or die("Query salah: " . mysqli_error());
				$nomor = 0;
				while ($myData = mysqli_fetch_array($myQry)) {
				$nomor++;
				$Kode = $myData['kdsurat'];
				  ?>
                  <tr>
                    <td width='2%'><?php echo $nomor; ?></td>
					 <td><?php echo $myData['kdsurat']; ?></td>
						<td><?php echo $myData['kdcabang']; ?></td>
						<td><?php echo $myData['namacab']; ?></td>
						<td><?php echo $myData['perihal'];?></td>
						<td><?php echo $myData['asal']; ?></td>
						<td><?php echo IndonesiaTgl($myData['tgl']); ?></td>
					<td>
					<a onclick="hapus_data('<?php echo $myData['kdsurat']; ?>');" class="btn btn-sm btn-danger">Delete</a>
					<a href="index.php?page=surat-edit&kdsurat=<?php echo $myData['kdsurat']; ?>" class="btn btn-sm btn-success">Edit Data</a>
					</td>

					<script>
						function hapus_data(data_Kode) {
							//var confirmation = confirm("Apakah Anda yakin ingin menghapus data ini?");
							//if (confirmation) {
								//window.location.href = "suratmasuk_delete.php?id=" + data_Kode;
							Swal.fire({
							  title: 'Apakah datanya akan di Hapus?',
							 // showDenyButton: true,
							  showCancelButton: true,
							  confirmButtonText: 'Hapus Data',
							  confirmButtonColor: '#CD5C5C',							  				  
							//  denyButtonText: `Don't save`,
							}).then((result) => {
							  /* Read more about isConfirmed, isDenied below */
							  if (result.isConfirmed) {
								window.location.href = "akad_delete.php?id=" + data_Kode;
							  }
							})
							
						}
					</script>

					
                  </tr>
                  <?php }?>
                  </tbody>
                  <tfoot>
                 
                  </tfoot>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
          <!-- /.col -->
        </div>
        <!-- /.row -->
      </div>
      <!-- /.container-fluid -->
    </section>		
	<?php
			$dataKode			=isset($_POST['textfield']) ? $_POST['textfield'] : '';
			$dataCabang			= isset($_POST['cmbcabang']) ? $_POST['cmbcabang'] : '';
			$datanama			= isset($_POST['txtnama']) ? $_POST['txtnama'] : '';
			$datajabatan		= isset($_POST['txtjabatan']) ? $_POST['txtjabatan'] : '';
			$datatanggalmulai	= isset($_POST['txttanggalmulai']) ? $_POST['txttanggalmulai']: '';
		?>
 <div class="modal fade" id="modal-lg">
         <div class="modal-dialog modal-lg">
          <div class="modal-content">
            <div class="modal-header">
              <h4 class="modal-title">Tambah Data Surat Masuk Kredit Konsumer</h4>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
			<!-- /.model formsnya -------------------------------->
			<form action="suratmasuk_add.php" method="post">			
		    <!-- /.berada diluar modal-body ---------------------->
			<div class="modal-body">									  
			<div class="form-group">
				<table>				
					<tr>
						  <td width="161"><strong>Nomor Surat</strong></td>
						  <td width="5"><b>:</b></td>
						  <td>
					<input name="txtnoakad" type="text" id="txtnoakad" class="form-control" size="35" placeholder="XXXX/KDCAB/MULTIGUNA/BLN/THN" maxlength="25" required />
				</td>
				</tr>
			 <script>
					$(document).ready(function() {
						// Fungsi untuk melakukan validasi txtnoakad
						function cekPK(txtnoakad) {
							$.ajax({
								url: "cek_nomorSurat.php",
								data: { txtnoakad: txtnoakad },
								method: "POST",
								dataType: "json",
								success: function(response) {
									if (response.ada_tidak == 1) {
										alert("Nomor Awal Pernjanjian Kredit Konsumer, Sudah Ada!");
										$("#txtnoakad").val('').focus();
									}
								}
							});
						}

						// Event onkeyup pada input txtnoakad
						$("#txtnoakad").on("keyup", function() {
							var txtnoakad = $(this).val();
							cekPK(txtnoakad);
						});
					});
				</script>				
						
						
						<tr>
						  <td><strong>Kode Cabang/Capem </strong></td>
						  <td><strong>:</strong></td>
						  <td><label>
							<select name="cmbcabang" class="form-control"required>
							  <option value="">Pilih</option>
							 <?php
										$dataSql = "SELECT * FROM cabang ORDER BY kdcabang";
										$dataQry = mysqli_query($koneksidb, $dataSql) or die("Gagal Query" . mysqli_error($koneksidb));
										while ($dataRow = mysqli_fetch_array($dataQry)) {
										  if ($datacabang == $dataRow['kdcabang']) {
											$cek = "selected";
										  } else {
											$cek = "";
										  }
										  echo "<option value='$dataRow[kdcabang]' $cek>[ $dataRow[kdcabang] ] $dataRow[namacab]</option>";
										}
										$sqlData = "";
										?>
									  </select>
									  <div id="cmbcabangError" class="error-message"></div>
									</td>
						</tr>
						<tr>
						  <td><b>Perihal</b></td>
						  <td><b>:</b></td>
						  <td><input name="txtnama" type="text" id="txtnama"  value="<?php echo $datanama; ?>" size="75" maxlength="255" class="form-control"required /></td>
						</tr>
						<tr>
						  <td><b>Asal Surat </b></td>
						  <td><b>:</b></td>
						  <td><input name="txtjabatan" type="text" id="txtjabatan" value="<?php echo $datajabatan; ?>" size="75" maxlength="255" class="form-control"required /></td>
						</tr>
						<tr>
						  <td><b>Tanggal Surat </b></td>
						  <td><b>:</b></td>
						  <td width="826"><input name="txttanggalmulai" type="text" id="tgl1" value="<?php echo $datatanggalmulai;?>" size="15" maxlength="25"class="form-control"required style="width: 150px;" />
						  
						</tr>
			  </table>
			  </div>			
            </div>
			<div class="modal-footer justify-content-between">
              <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
              <button type="submit" name="btnSimpan" class="btn btn-primary">Save changes</button>
            </div>
			</form>
			<!-- /.tutup formsnya ------------------------>
            
          </div>
          <!-- /.modal-content -->
        </div>
        <!-- /.modal-dialog -->
      </div>
	  
