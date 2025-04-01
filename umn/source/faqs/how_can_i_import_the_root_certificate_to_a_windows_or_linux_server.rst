:original_name: opengauss_01_0088.html

.. _opengauss_01_0088:

How Can I Import the Root Certificate to a Windows or Linux Server?
===================================================================

Importing the Root Certificate to a Windows Server
--------------------------------------------------

#. Press **Win+R** to open the **Run** dialog box, enter **MMC**, and press **Enter**.
#. On the displayed console, choose **File** > **Add/Remove Snap-in**.
#. In the **Available snap-ins** pane to the left of the displayed **Add or Remove Snap-ins** dialog box, select **Certificates** and click **Add**.
#. In the displayed **Certificates snap-in** dialog box, select **Computer account** and click **Next**.
#. In the displayed **Select Computer** dialog box, click **Finish**.
#. In the **Add or Remove Snap-ins** dialog box, click **OK**.
#. On the console, double-click **Certificates**.
#. Right-click **Trusted Root Certification Authorities** and choose **All Tasks** > **Import**.
#. In the displayed **Certificate Import Wizard** dialog box, click **Next**.
#. Click **Browse** to change the file type to **All Files (*.*)**.
#. Locate the downloaded root certificate (a ca.pem file) and click **Open**. Then, click **Next**.

   .. important::

      You must change the file type to **All Files (*.*)** because **.pem** is not a standard certificate extension name.

#. Click **Next**.
#. Click **Finish**.
#. Click **OK** to complete the import of the root certificate.

Importing the Root Certificate to a Linux Server
------------------------------------------------

You can use a connection tool (such as WinSCP or PuTTY) to upload the certificate to any directory on a Linux Server.
