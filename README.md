# GUI_Wrapper

#region Source: Startup.pfs
#----------------------------------------------
#region Import Assemblies
#----------------------------------------------
[void][Reflection.Assembly]::Load("System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
[void][Reflection.Assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
[void][Reflection.Assembly]::Load("System.Drawing, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a")
[void][Reflection.Assembly]::Load("mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
[void][Reflection.Assembly]::Load("System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
[void][Reflection.Assembly]::Load("System.Xml, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
[void][Reflection.Assembly]::Load("System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a")
#endregion Import Assemblies

#Define a Param block to use custom parameters in the project
#Param ($CustomParameter)

function Main {
    Param ([String]$Commandline)
    if((Call-MainForm_pff) -eq "OK")
    {
      
    }
    $global:ExitCode = 0 #Set the exit code for the Packager
}
#endregion Source: Startup.pfs

#region Source: Globals.ps1
	# Default Values
	$CONTROL_SIZE_X = 385
	$CONTROL_SIZE_Y = 20
	$CONTROL_LOCATION_X = 3
	$CONTROL_LOCATION_Y = 10
	$CONTROL_LOCATION_Y_INCREMENT = ($CONTROL_SIZE_Y + 5)
	
	#region Code Template Strings
	#   <0> = Parameter Name
	$CODE_FRMOBJ_COMBO = @'
    $label_<0> = New-Object 'System.Windows.Forms.Label'
    $combobox_<0> = New-Object 'System.Windows.Forms.ComboBox'

'@
	
	$CODE_FRMOBJ_DIAL = @'
    $label_<0> = New-Object 'System.Windows.Forms.Label'
    $dial_<0> = New-Object 'System.Windows.Forms.NumericUpDown'

'@
	
	$CODE_FRMOBJ_CHECKBOX = @'
    $checkbox_<0> = New-Object 'System.Windows.Forms.CheckBox'

'@
	
	$CODE_FRMOBJ_TEXTBOX = @'
    $label_<0> = New-Object 'System.Windows.Forms.Label'
    $textbox_<0> = New-Object 'System.Windows.Forms.TextBox'

'@
	
	# <#AddControlsToForm#>
	#   <0> = Parameter Name
	#   <1> = Parameter tooltip Message
	#   <2> = Tab Index
	#   <3> = Location X (3)
	#   <4> = Location Y
	#   <5> = Size X    (385)
	#   <6> = Size Y    (25)
	#   <7> = Default Value
	
	$CODE_ADDCTRL_LABEL = @'

    # label_<0>
    $formpanel.Controls.Add($label_<0>)  #Add control to panel
    $label_<0>.Anchor = 'Top, Left, Right'
    $label_<0>.Font = "Microsoft Sans Serif, 7.8pt, style=Bold, Underline"
    $label_<0>.Location = '<3>, <4>'
    $label_<0>.Name = "label_<0>"
    $label_<0>.Size = '<5>, <6>'
    $label_<0>.TabIndex = <2>
    $label_<0>.Text = "<0>"
    $label_<0>.TextAlign = 'MiddleLeft'

'@
	
	$CODE_ADDCTRL_COMBO = @'

    # combobox_<0>
    $formpanel.Controls.Add($combobox_<0>)   #Add control to panel
    $combobox_<0>.Anchor = 'Top, Left, Right'
    $combobox_<0>.DropDownStyle = 'DropDownList'
    $combobox_<0>.FormattingEnabled = $True
    $tooltip.SetToolTip($combobox_<0>, "<1>")
    $combobox_<0>.Location = '<3>, <4>'
    $combobox_<0>.Name = "combobox_<0>"
    $combobox_<0>.Size = '<5>, <6>'
    $combobox_<0>.TabIndex = <2>
    $combobox_<0>.Text = '<7>'

'@
	
	$CODE_ADDCTRL_DIAL = @'

    # dial_<0>
    $formpanel.Controls.Add($dial_<0>)   #Add control to panel
    $dial_<0>.Anchor = 'Top, Left, Right'
    $dial_<0>.Maximum = 32000
    $tooltip.SetToolTip($dial_<0>, "<1>")
    $dial_<0>.Location = '<3>, <4>'
    $dial_<0>.Name = "dial_<0>"
    $dial_<0>.Size = '<5>, <6>'
    $dial_<0>.TabIndex = <2>
    $dial_<0>.Value = '<7>'

'@
	
	$CODE_ADDCTRL_CHECKBOX = @'

    # checkbox_<0>
    $formpanel.Controls.Add($checkbox_<0>)      #Add control to panel
    $checkbox_<0>.Anchor = 'Top, Left, Right'
    $checkbox_<0>.Location = '<3>, <4>'
    $checkbox_<0>.Name = "checkbox_<0>"
    $tooltip.SetToolTip($checkbox_<0>, "<1>")
    $checkbox_<0>.Size = '<5>, <6>'
    $checkbox_<0>.TabIndex = <2>
    $checkbox_<0>.Text = "<0>"
    $checkbox_<0>.UseVisualStyleBackColor = $True

'@
	
	$CODE_ADDCTRL_TEXTBOX = @'

    # textbox_<0>
    $formpanel.Controls.Add($textbox_<0>)
    $textbox_<0>.Anchor = 'Top, Left, Right'
    $tooltip.SetToolTip($textbox_<0>, "<1>")
    $textbox_<0>.Location = '<3>, <4>'
    $textbox_<0>.Name = "textbox_<0>"
    $textbox_<0>.Size = '<5>, <6>'
    $textbox_<0>.TabIndex = <2>
    $textbox_<0>.Text = "<7>"

'@
	
	# Combo value addition code
	# <0> - Parameter Name
	# <1> - Combo Value
	$CODE_ADDCOMBOVALUE = @'

    [void]$combobox_<0>.AutoCompleteCustomSource.Add("<1>")
    [void]$combobox_<0>.Items.Add("<1>")
'@
	#endregion
	
	#region Project Template Strings
	# Globals.ps1
	# <#FunctionIsExternal#>
	# <#FunctionName#>
	# <#FunctionPath#>
	# <#ParamTypeVariables#>
	# <#MandatoryParams#>
	
	# ScriptWrapperGUIOutputTemplate.pfproj
	#
	# <#FunctionName#>
	# <#AddProjControlsToForm#>
	# 
	#
	
	$PROJ_ADDCTRL_COMBOITEM1 = @'

<Item type="System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"><0></Item>
'@
	$PROJ_ADDCTRL_COMBOITEM2 = @'

<Item type="System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"><0></Item>
'@
	$PROJ_ADDCTRL_COMBOVALUES = @'

<Property name="AutoCompleteCustomSource">
          <#ComboItems1#>
        </Property>
        <Property name="Items">
          <#ComboItems2#>
        </Property>

'@
	
	$PROJ_ADDCTRL_LABEL = @'

      <Object type="System.Windows.Forms.Label, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" name="label_<0>" children="Controls">
        <Property name="Anchor">Top, Left, Right</Property>
        <Property name="Font">Microsoft Sans Serif, 7.8pt, style=Bold, Underline</Property>
        <Property name="Location"><3>, <4></Property>
        <Property name="Name">label_<0></Property>
        <Property name="Size"><5>, <6></Property>
        <Property name="TabIndex"><2></Property>
        <Property name="Text"><0></Property>
        <Property name="TextAlign">MiddleLeft</Property>
      </Object>

'@
	
	$PROJ_ADDCTRL_COMBO = @'

      <Object type="System.Windows.Forms.ComboBox, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" name="combobox_<0>" children="Controls">
        <Property name="Anchor">Top, Left, Right</Property>
        <Property name="DropDownStyle">DropDownList</Property>
        <Property name="FormattingEnabled">True</Property>
        <Property name="Location"><3>, <4></Property>
        <Property name="Name">combobox_<0></Property>
        <Property name="Size"><5>, <6></Property>
        <Property name="TabIndex"><2></Property>
        <Property name="Text"><7></Property>
        <Property name="ToolTip" extended="True" provider="tooltip"><1></Property>
        <#ComboValues#>
      </Object>

'@
	
	$PROJ_ADDCTRL_DIAL = @'

      <Object type="System.Windows.Forms.NumericUpDown, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" name="dial_<0>" children="Controls">
        <Property name="Anchor">Top, Left, Right</Property>
        <Property name="Location"><3>, <4></Property>
        <Property name="Name">dial_<0></Property>
        <Property name="Size"><5>, <6></Property>
        <Property name="TabIndex"><2></Property>
        <Property name="Value"><7></Property>
        <Property name="Maximum">32000</Property>
        <Property name="ToolTip" extended="True" provider="tooltip"><1></Property>
        <Property name="UseVisualStyleBackColor">True</Property>
      </Object>

'@
	
	$PROJ_ADDCTRL_CHECKBOX = @'

      <Object type="System.Windows.Forms.CheckBox, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" name="checkbox_<0>" children="Controls">
        <Property name="Anchor">Top, Left, Right</Property>
        <Property name="Location"><3>, <4></Property>
        <Property name="Name">checkbox_<0></Property>
        <Property name="Size"><5>, <6></Property>
        <Property name="TabIndex"><2></Property>
        <Property name="Text"><0></Property>
        <Property name="ToolTip" extended="True" provider="tooltip"><1></Property>
        <Property name="UseVisualStyleBackColor">True</Property>
      </Object>

'@
	
	$PROJ_ADDCTRL_TEXTBOX = @'

      <Object type="System.Windows.Forms.TextBox, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" name="textbox_<0>" children="Controls">
        <Property name="Anchor">Top, Left, Right</Property>
        <Property name="Location"><3>, <4></Property>
        <Property name="Name">textbox_<0></Property>
        <Property name="Size"><5>, <6></Property>
        <Property name="TabIndex"><2></Property>
        <Property name="Text"><7></Property>
        <Property name="ToolTip" extended="True" provider="tooltip"><1></Property>
      </Object>

'@
	#endregion
	
	Function Get-FunctionParameters
	{
	    <#
	        .SYNOPSIS
	            Return all parameter elements from a function param() block.
	       
	           	Zachary Loeber
	        	
	        	THIS CODE IS MADE AVAILABLE AS IS, WITHOUT WARRANTY OF ANY KIND. THE ENTIRE 
	        	RISK OF THE USE OR THE RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.
	        	
	        	Version 1.1 - 01/27/2014
	        .DESCRIPTION
	            This function will return all the parameters defined in a param() portion of a
	            script as well as any default values, variable type information, HelpMessage 
	            text, ValidateSet items, and mandatory settings if present.
	        .PARAMETER ScriptBlock
	            A scriptblock containing a parameter set
	        .EXAMPLE
	            Get-FunctionParameter $Script
	        .EXAMPLE
	            $testscript = Get-Content 'SampleFunction.ps1' -Raw
	            $scriptparams = get-functionparameters -ScriptBlock $testscript
	        .NOTES
	            Author: Zachary Loeber
	
	            Version History:
	            1.1 - 01/27/2014
	                - Added ability to return mandatory info as well as validateset
	                  values.
	            1.0 - 05/31/2013
	                - First release
	        .LINK 
	            http://www.the-little-things.net 
	        .LINK
	            http://nl.linkedin.com/in/zloeber
	    #>
	    [CmdletBinding()]
	    param
	    (
	        [Parameter( Mandatory=$true,
	                    ValueFromPipeline=$false,
	                    HelpMessage="A scriptblock containing a parameter set.")]
	        [String]$ScriptBlock
	    )
	    BEGIN
	    {
	        # Turn our string into a scriptblock
	    	$ScriptBlock = [Scriptblock]::Create($ScriptBlock)
	        $paramset = @()
	        
	        # Tokenize the script
	        $tokens = [Management.Automation.PSParser]::Tokenize($ScriptBlock, [ref]$null) | 
	                  Where {$_.Type -ne 'NewLine'}
	    }
	    PROCESS{}
	    END
	    {
	        # First Pass - Grab all tokens between the first param block.
	        $paramsearch = $false
	        $groupstart = 0
	        $groupend = 0
	        for ($i = 0; $i -lt $tokens.Count; $i++) {
	            if (!$paramsearch)
	            {
	                if ($tokens[$i].Content -eq "param" )
	                {
	                    $paramsearch = $true
	                }
	            }
	            if ($paramsearch)
	            {
	                if (($tokens[$i].Type -eq "GroupStart") -and ($tokens[$i].Content -eq '(') )
	                {
	                    $groupstart++
	                }
	                if (($tokens[$i].Type -eq "GroupEnd") -and ($tokens[$i].Content -eq ')') )
	                {
	                    $groupend++
	                }
	                if (($groupstart -ge 1) -and ($groupstart -eq $groupend))
	                {
	                    $paramsearch = $false
	                }
	                $isparameter = $false
	                $paramdatatype = ''
	                $defaultvalue = ''
	                if (($tokens[$i].Type -eq 'Variable') -and ($tokens[($i-1)].Content -ne '='))
	                {
	                    if ((($groupstart - $groupend) -eq 1))
	                    {
	                        $isparameter = $true
	                    }
	                }
	                if ($isparameter)
	                {
	                    # if assigned, get the parameter default value
	                    if (($tokens[($i+1)].Type -eq 'Operator') -and ($tokens[($i+1)].Content -eq '='))
	                    {
	                        $defaultvalue = $tokens[($i+2)].Content
	                    }
	                    
	                    # Get the parameter data type
	                    if (($tokens[($i-1)].Type -eq 'Type'))
	                    {
	                        $paramdatatype = $tokens[($i-1)].Content
	                    }
	                }
	                $objprop = @{ 'Type' = $tokens[$i].Type;
	                              'Content' = $tokens[$i].Content;
	                              'IsParameter' = $isparameter;
	                              'ParameterVariableType' = ($paramdatatype -replace '[^A-Za-z]','');
	                              'ParamDefaultValue' = $defaultvalue;
	                              'HelpMessage' = '';
	                              'Mandatory' = $false;
	                              'ValidateSet' = @();
	                            }
	                $newobj = New-Object PSObject -Property $objprop
	                $paramset = $paramset + $newobj
	            }
	        }
	
	        # Second Pass - Find all helpmessage and other parameter attributes
	        #               associated with each parameter in the block
	        $i=0
	        Foreach ($param_item in $paramset)
	        {
	            if ($param_item.IsParameter)
	            {
	                # Once we find a parameter we essentially do a backward search
	                # until we either hit another parameter or the beginning of the
	                # entire param block.
	                $lookforhelptext = $true
	                $helpsearch = $i
	                While ($lookforhelptext)
	                {
	                    $helpsearch--
	                    if(($paramset[$helpsearch].IsParameter) -or 
	                       (($paramset[$helpsearch].Type -eq 'Keyword') -and 
	                        ($paramset[$helpsearch].Content -eq 'param')) -or 
	                       ($helpsearch -eq 0))
	                    {
	                        $lookforhelptext = $false
	                    }
	                    # Get help message
	                    elseif (($paramset[$helpsearch].Content -eq 'HelpMessage') -and 
	                            ($paramset[$helpsearch].Type -eq 'Member'))
	                    {
	                        $param_item.HelpMessage = $paramset[($helpsearch+2)].Content
	                    }
	                    # Get mandatory state
	                    elseif (($paramset[$helpsearch].Content -eq 'Mandatory') -and 
	                            ($paramset[$helpsearch].Type -eq 'Member'))
	                    {
	                        $param_item.Mandatory = ($paramset[($helpsearch+2)].Content -eq 'true')
	                    }
	                    # Get our validateset items
	                    elseif (($paramset[$helpsearch].Content -eq 'ValidateSet') -and 
	                            ($paramset[$helpsearch].Type -eq 'Attribute'))
	                    {
	                        $foundallinset = $false
	                        $vssetsearch = $helpsearch+2
	                        do
	                        {
	                            if ($paramset[$vssetsearch].Type -ne 'Operator')
	                            {
	                                $param_item.ValidateSet += $paramset[$vssetsearch].Content
	                            }
	                            $vssetsearch++
	                        } Until ($paramset[$vssetsearch].Type -eq 'GroupEnd')
	                    }
	                }
	            }
	            $i++
	        }
	        $paramset | where {($_.IsParameter)}
	    }
	}
	
	function Get-ScriptDirectory
	{ 
		if($hostinvocation -ne $null)
		{
			Split-Path $hostinvocation.MyCommand.path
		}
		else
		{
			Split-Path $script:MyInvocation.MyCommand.Path
		}
	}
#endregion Source: Globals.ps1

#region Source: MainForm.pff
function Call-MainForm_pff
{
	#----------------------------------------------
	#region Import the Assemblies
	#----------------------------------------------
	[void][reflection.assembly]::Load("mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.Drawing, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a")
	[void][reflection.assembly]::Load("System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.Xml, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a")
	[void][reflection.assembly]::Load("System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	[void][reflection.assembly]::Load("System.ServiceProcess, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a")
	#endregion Import Assemblies

	#----------------------------------------------
	#region Generated Form Objects
	#----------------------------------------------
	[System.Windows.Forms.Application]::EnableVisualStyles()
	$CreateFormWrapper = New-Object 'System.Windows.Forms.Form'
	$labelVersion03betaAuthorZ = New-Object 'System.Windows.Forms.Label'
	$linklabelHttpwwwthelittlethin = New-Object 'System.Windows.Forms.LinkLabel'
	$groupboxScriptInput = New-Object 'System.Windows.Forms.GroupBox'
	$textboxScriptFunction = New-Object 'System.Windows.Forms.TextBox'
	$radiobuttonUseText = New-Object 'System.Windows.Forms.RadioButton'
	$radiobuttonUseFile = New-Object 'System.Windows.Forms.RadioButton'
	$textboxInputScript = New-Object 'System.Windows.Forms.TextBox'
	$buttonBrowseInput = New-Object 'System.Windows.Forms.Button'
	$groupboxScriptOutput = New-Object 'System.Windows.Forms.GroupBox'
	$checkboxIncludeVerboseParame = New-Object 'System.Windows.Forms.CheckBox'
	$checkboxLaunchAsSepProcess = New-Object 'System.Windows.Forms.CheckBox'
	$buttonBrowseFolder = New-Object 'System.Windows.Forms.Button'
	$textboxProjectFolder = New-Object 'System.Windows.Forms.TextBox'
	$checkboxCreateProject = New-Object 'System.Windows.Forms.CheckBox'
	$checkboxExcludeFunctionSource = New-Object 'System.Windows.Forms.CheckBox'
	$checkboxUseDatagridOutput = New-Object 'System.Windows.Forms.CheckBox'
	$checkbox_LaunchGUI = New-Object 'System.Windows.Forms.CheckBox'
	$textboxFile = New-Object 'System.Windows.Forms.TextBox'
	$labelSaveAs = New-Object 'System.Windows.Forms.Label'
	$buttonBrowse = New-Object 'System.Windows.Forms.Button'
	$buttonGo = New-Object 'System.Windows.Forms.Button'
	$openfiledialog1 = New-Object 'System.Windows.Forms.OpenFileDialog'
	$timerFadeIn = New-Object 'System.Windows.Forms.Timer'
	$tooltip1 = New-Object 'System.Windows.Forms.ToolTip'
	$folderbrowserdialog1 = New-Object 'System.Windows.Forms.FolderBrowserDialog'
	$InitialFormWindowState = New-Object 'System.Windows.Forms.FormWindowState'
	#endregion Generated Form Objects

	#----------------------------------------------
	# User Generated Script
	#----------------------------------------------
	$FormEvent_Load={
	}
	
	$buttonBrowse_Click={
	    $openfiledialog1.CheckFileExists = $false
		if($openfiledialog1.ShowDialog() -eq 'OK')
		{
			$textboxFile.Text = $openfiledialog1.FileName
		}
	}
	
	$form1_FadeInLoad={
		#Start the Timer to Fade In
		$timerFadeIn.Start()
		$CreateFormWrapper.Opacity = 0
	}
	
	$timerFadeIn_Tick={
		#Can you see me now?
		if($CreateFormWrapper.Opacity -lt 1)
		{
			$CreateFormWrapper.Opacity += 0.1
			
			if($CreateFormWrapper.Opacity -ge 1)
			{
				#Stop the timer once we are 100% visible
				$timerFadeIn.Stop()
			}
		}
	}
	
	$buttonGo_Click={
	    if ((! $checkbox_LaunchGUI.Checked) -and
	        (($textboxFile.Text -eq '') -and ($radiobuttonUseText.Checked)) -or
	        (($textboxInputScript.Text -eq '') -and ($radiobuttonUseFile.Checked)))
	    {
	        #[void][reflection.assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	        [void][System.Windows.Forms.MessageBox]::Show("Filename Missing and you have not selected to run the gui.`n`r No output will be generated.","What are you trying to do?")
		}
	    else
	    {
	        $InputScript = ''
	        if ($radiobuttonUseText.Checked)
	        {
	            $InputScript = $textboxScriptFunction.Text
			}
	        else
	        {
	            if ($textboxInputScript.Text -ne '')
	            {
	                [string]$InputScript = Get-Content -Path  $textboxInputScript.Text -Raw
				}
			}
	        if ($InputScript -ne '')
	        {
	            # Initialize some variables
	            $FirstComment = ''
	            $DefaultParamSet = ''
	            
	            $ErrorsEncountered = $false
	            $ForceExternalFunctionCall = $false
	            $scriptparams = @(Get-FunctionParameters -ScriptBlock $InputScript)
	            if ($checkboxIncludeVerboseParame.Checked)
	            {
	                $verboseparam = @{
	                    'Type' = 'Variable';
	                    'Content' = 'Verbose';
	                    'IsParameter' = $true;
	                    'ParameterVariableType' = 'Boolean';
	                    'ParamDefaultValue' = $false;
	                    'HelpMessage' = 'Return verbose output.';
	                    'Mandatory' = $false;
	                    'ValidateSet' = @();
					}
	                $scriptparams += New-Object psobject -Property $verboseparam
				}
	            if ($scriptparams.Count -ge 1)
	            {
	                $ScriptBlock = [Scriptblock]::Create($InputScript)
	
	                # Tokenize the script
	                $tokens = [Management.Automation.PSParser]::Tokenize($ScriptBlock, [ref]$null) | Where {$_.Type -ne 'NewLine'}
	                
	                # Get the default paramset if it exists
	                for ($i=0; $i -lt $tokens.Count; $i++) {
	                	if (($tokens[$i].Type -eq 'Member') -and ($tokens[$i].Content -eq 'DefaultParametersetName'))
	                    {
	                        $DefaultParamSet = $tokens[$i+2]
	                    }
	                }
	                
	                # Get the first comment (assuming it is the comment based help
	                $FirstComment = $tokens | Where {$_.Type -eq 'Comment'} | Select -First 1 | %{$_.Content}
	
	                $FirstComment = $FirstComment -replace '<#',''
	                $FirstComment = $FirstComment -replace '#>',''
	                $FirstComment = $FirstComment -replace '#',''
	
	                # Get the first function name if it exists
	                $i = 0
	                $SearchingForFunctionName = $true
	                $FunctionName = 'YourFunction'
	                While ($SearchingForFunctionName -and ($i -lt $tokens.Count))
	                {
	                    if ($tokens[$i].Type -eq 'Keyword')
	                    {
	                        if ($tokens[$i].Content -eq 'Function')
	                        {
	                            $FunctionName = ($tokens[($i+1)]).Content
							}
	                        else
	                        {
	                            $ForceExternalFunctionCall = $true
							}
	                        $SearchingForFunctionName = $false
	                    }
	                    $i++
	                }
	
	                $ParamTypeVariables = ''
	                $MandatoryParams = ''
	                $GeneratedFormObjects = ''
	                $AddControlsToForm = ''
	
	                $AddProjControlsToForm = ''
	                $ProjComboItems1 = ''
	                $ProjComboItems2 = ''
	                $TabIndex = 0
	                
	                ForEach ($scriptparam in $scriptparams)
	                {
	                    $paramname = $scriptparam.Content
	                    
	                    # Update our mandatory hash
	                    $MandatoryParams = $MandatoryParams + 
	                                       '$MandatoryParams.Add("' + $paramname + '",$' + $scriptparam.Mandatory + ")`n`r"
	                    
	                    Switch ($scriptparam.ParameterVariableType) 
	                    {
	                        { @("Switch", "Boolean", "Bool") -contains $_ } {
	                            # Create a checkbox
	                            $ParamTypeVariables = $ParamTypeVariables + 
	                                                  '    $FunctionParamTypes.Add("checkbox_' + 
	                                                  $paramname + '","checkbox")' + "`n`r"
	                            $GeneratedFormObjects = $GeneratedFormObjects + $CODE_FRMOBJ_CHECKBOX -replace '<0>',$paramname
	                            
	                            $AddControlsToForm = $AddControlsToForm + 
	                                                 ($CODE_ADDCTRL_CHECKBOX -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y)
	                            $AddProjControlsToForm = $AddProjControlsToForm + 
	                                                 ($PROJ_ADDCTRL_CHECKBOX -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y)
	                            $TabIndex++
							}
	                        'int' {
	                            # Adding a label no matter what
	                            $AddControlsToForm = $AddControlsToForm + 
	                                             ($CODE_ADDCTRL_LABEL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',($TabIndex + 100) -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y)
	                            $AddProjControlsToForm = $AddProjControlsToForm + 
	                                             ($PROJ_ADDCTRL_LABEL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',($TabIndex + 100) -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y)
	                            $TabIndex++
	                            
	                            # Create a dial
	                            $ParamTypeVariables = $ParamTypeVariables + 
	                                                  '    $FunctionParamTypes.Add("dial_' + 
	                                                  $paramname + '","int")' + "`n`r"
	                            $GeneratedFormObjects = $GeneratedFormObjects + $CODE_FRMOBJ_DIAL -replace '<0>',$paramname
	
	                            $AddControlsToForm = $AddControlsToForm + 
	                                             ($CODE_ADDCTRL_DIAL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',$TabIndex -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y -replace
	                                               '<7>',$scriptparam.ParamDefaultValue)
	                            $AddProjControlsToForm = $AddProjControlsToForm +
	                                             ($PROJ_ADDCTRL_DIAL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',$TabIndex -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y -replace
	                                               '<7>',$scriptparam.ParamDefaultValue)
	                            $TabIndex++
							}
	                        'String' {
	                            # Adding a label no matter what
	                            $AddControlsToForm = $AddControlsToForm + 
	                                             ($CODE_ADDCTRL_LABEL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',($TabIndex + 100) -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y)
	                            $AddProjControlsToForm = $AddProjControlsToForm + 
	                                             ($PROJ_ADDCTRL_LABEL -replace 
	                                               '<0>',$paramname -replace
	                                               '<1>',$scriptparam.HelpMessage -replace
	                                               '<2>',($TabIndex + 100) -replace
	                                               '<3>',$CONTROL_LOCATION_X -replace
	                                               '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                               '<5>',$CONTROL_SIZE_X -replace
	                                               '<6>',$CONTROL_SIZE_Y)
	                            $TabIndex++
	                            
	                            # Check if we are creating a textbox or combobox
	                            if (($scriptparam.ValidateSet).Count -gt 0)
	                            {
	                                # Create a combobox
	                                $ParamTypeVariables = $ParamTypeVariables + 
	                                                      '    $FunctionParamTypes.Add("combobox_' + 
	                                                      $paramname + '","combobox")' + "`n`r"
	                                $GeneratedFormObjects = $GeneratedFormObjects + $CODE_FRMOBJ_COMBO -replace '<0>',$paramname
	                                $ProjComboItems1 = ''
	                                $ProjComboItems2 = ''
	                                
	                                # Add the combobox values
	                                foreach ($combovalue in ($scriptparam.ValidateSet))
	                                {
	                                    $AddControlsToForm = $AddControlsToForm + ($CODE_ADDCOMBOVALUE -replace '<0>',$paramname -replace '<1>',$combovalue)
	                                    $ProjComboItems1 = $ProjComboItems1 + ($PROJ_ADDCTRL_COMBOITEM1 -replace '<0>',$combovalue)
	                                    $ProjComboItems2 = $ProjComboItems2 + ($PROJ_ADDCTRL_COMBOITEM2 -replace '<0>',$combovalue)
	                                    
									}
	                                $ProjComboItems = $PROJ_ADDCTRL_COMBOVALUES -replace 
	                                                      '<#ComboItems1#>',$ProjComboItems1 -replace
	                                                      '<#ComboItems2#>',$ProjComboItems2 
	                                $AddControlsToForm = $AddControlsToForm + 
	                                                 ($CODE_ADDCTRL_COMBO -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y -replace
	                                                   '<7>',$scriptparam.ParamDefaultValue)
	                                $AddProjControlsToForm = $AddProjControlsToForm +
	                                                 ($PROJ_ADDCTRL_COMBO -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y -replace
	                                                   '<7>',$scriptparam.ParamDefaultValue -replace
	                                                   '<#ComboValues#>',$ProjComboItems)
	                                $TabIndex++
								}
	                            else
	                            {
	                                # Create a textbox
	                                $ParamTypeVariables = $ParamTypeVariables + 
	                                                      '    $FunctionParamTypes.Add("textbox_' +
	                                                      $paramname + '","string")' + "`n`r"
	                                $GeneratedFormObjects = $GeneratedFormObjects + $CODE_FRMOBJ_TEXTBOX -replace '<0>',$paramname
	                                
	                                $AddControlsToForm = $AddControlsToForm + 
	                                                 ($CODE_ADDCTRL_TEXTBOX -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y -replace
	                                                   '<7>',$scriptparam.ParamDefaultValue)
	                                $AddProjControlsToForm = $AddProjControlsToForm + 
	                                                 ($PROJ_ADDCTRL_TEXTBOX -replace 
	                                                   '<0>',$paramname -replace
	                                                   '<1>',$scriptparam.HelpMessage -replace
	                                                   '<2>',$TabIndex -replace
	                                                   '<3>',$CONTROL_LOCATION_X -replace
	                                                   '<4>',($CONTROL_LOCATION_Y + ($CONTROL_LOCATION_Y_INCREMENT * $TabIndex)) -replace
	                                                   '<5>',$CONTROL_SIZE_X -replace
	                                                   '<6>',$CONTROL_SIZE_Y -replace
	                                                   '<7>',$scriptparam.ParamDefaultValue)
	                                $TabIndex++
								}
							}
						}
					}
	                
	                #Update our script template
	                $ScriptDirectory = Get-ScriptDirectory
	                $ScriptTemplate = Get-Content "$ScriptDirectory\Templates\ScriptFormTemplate.ps1" -Raw
	
	                $ScriptTemplate = $ScriptTemplate -replace '<#ParamTypeVariables#>',$ParamTypeVariables
	                $ScriptTemplate = $ScriptTemplate -replace '<#MandatoryParams#>',$MandatoryParams
	                $ScriptTemplate = $ScriptTemplate -replace '<#GeneratedFormObjects#>',$GeneratedFormObjects
	                $ScriptTemplate = $ScriptTemplate -replace '<#AddControlsToForm#>',$AddControlsToForm
	                $ScriptTemplate = $ScriptTemplate -replace '<#FunctionName#>',$FunctionName
	                $ScriptTemplate = $ScriptTemplate -replace '<#FunctionPath#>',$textboxInputScript.Text
	                $ScriptTemplate = $ScriptTemplate -replace '<#CommentBasedHelp#>',$FirstComment
	                $ScriptTemplate = $ScriptTemplate -replace '<#ExecuteButtonTabIndex#>',$TabIndex
	                
	                if ($checkboxUseDatagridOutput.Checked)
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#OutputToGrid#>','$true'
					}
	                else
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#OutputToGrid#>','$false'
					}
	                
	                if ($checkboxLaunchAsSepProcess.Checked)
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#LaunchAsSeperateProcess#>','$true'
					}
	                else
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#LaunchAsSeperateProcess#>','$false'
					}
	                
	                if (($checkboxExcludeFunctionSource.Checked) -or ($ForceExternalFunctionCall))
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#FunctionIsExternal#>','$true'
					}
	                else
	                {
	                    $ScriptTemplate = $ScriptTemplate -replace '<#FunctionIsExternal#>','$false'
	                    $ScriptTemplate = $ScriptTemplate -replace '<#FunctionBlock#>',$InputScript
					}
	                if ($textboxFile.Text -ne '')
	                {
	                    $ScriptTemplate | Out-File -FilePath $textboxFile.Text
	                    #[void][reflection.assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	                    [void][System.Windows.Forms.MessageBox]::Show("GUI wrapper script created!","Success!")
					}
	                if ($checkbox_LaunchGUI.Checked)
	                {
	                    $OutputScriptBlock = [Scriptblock]::Create($ScriptTemplate)
	                    Invoke-Command $OutputScriptBlock
					}
	                if ($checkboxCreateProject.Checked)
	                {
	                    # Create the project files
	                    $ProjGlobals = Get-Content "$ScriptDirectory\Templates\Globals.ps1" -Raw
	                    $ProjGlobals = $ProjGlobals -replace '<#ParamTypeVariables#>',$ParamTypeVariables
	                    $ProjGlobals = $ProjGlobals -replace '<#MandatoryParams#>',$MandatoryParams
	                    $ProjGlobals = $ProjGlobals -replace '<#FunctionName#>',$FunctionName
	                    $ProjGlobals = $ProjGlobals -replace '<#FunctionPath#>',$textboxInputScript.Text                    
	                    
	                    $ProjForm = Get-Content "$ScriptDirectory\Templates\ExampleOutputForm.pff" -Raw
	                    $ProjForm = $ProjForm -replace '<#AddProjControlsToForm#>',$AddProjControlsToForm
	                    $ProjForm = $ProjForm -replace '<#FunctionName#>',$FunctionName
	                    $ProjForm = $ProjForm -replace '<#CommentBasedHelp#>',$FirstComment
	                    $ProjForm = $ProjForm -replace '<#ExecuteButtonTabIndex#>',$TabIndex
	                    
	                    if ($checkboxUseDatagridOutput.Checked)
	                    {
	                        $ProjGlobals = $ProjGlobals -replace '<#OutputToGrid#>','$true'
	    				}
	                    else
	                    {
	                        $ProjGlobals = $ProjGlobals -replace '<#OutputToGrid#>','$false'
	    				}
	                    
	                    if (($checkboxExcludeFunctionSource.Checked) -or ($ForceExternalFunctionCall))
	                    {
	                        $ProjGlobals = $ProjGlobals -replace '<#FunctionIsExternal#>','$true'
	    				}
	                    else
	                    {
	                        $ProjGlobals = $ProjGlobals -replace '<#FunctionIsExternal#>','$false'
	                        $ProjGlobals = $ProjGlobals -replace '<#FunctionBlock#>',$InputScript
	    				}
	                    $OutputScriptDirectory = $ScriptDirectory + '\PoshStudioOutputProject'
	                    if ($textboxProjectFolder.Text -ne '')
	                    {
	                        $OutputScriptDirectory = $textboxProjectFolder.Text
						}
	                    $ProjGlobals | Out-File -FilePath "$OutputScriptDirectory\Globals.ps1"
	                    $ProjForm | Out-File -FilePath "$OutputScriptDirectory\ExampleOutputForm.pff"
	                    Copy-Item "$ScriptDirectory\templates\ScriptWrapperGUIOutputTemplate.pfproj" "$OutputScriptDirectory\ScriptWrapperGUIOutputTemplate.pfproj"
	                    Copy-Item "$ScriptDirectory\templates\ScriptWrapperGUIOutputTemplate.pfprojs" "$OutputScriptDirectory\ScriptWrapperGUIOutputTemplate.pfprojs"
	                    Copy-Item "$ScriptDirectory\templates\Startup.pfs" "$OutputScriptDirectory\Startup.pfs"
	                    
	                    #[void][reflection.assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	                    [void][System.Windows.Forms.MessageBox]::Show("Project files for Powershell studio 2012 have been created in $OutputScriptDirectory","Complete!")
	                }
				}
	            else
	            {
	                 #[void][reflection.assembly]::Load("System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
	                [void][System.Windows.Forms.MessageBox]::Show("No parameters found so there is nothing to do.","Warning!")
	                $ErrorsEncountered = $true
				}
			}
		}
	}
	
	$buttonBrowseInput_Click={
	    $openfiledialog1.CheckFileExists = $true
		if($openfiledialog1.ShowDialog() -eq 'OK')
		{
			$textboxInputScript.Text = $openfiledialog1.FileName
		}
	}
	
	$radiobuttonUseText_CheckedChanged={
	    if ($radiobuttonUseText.Checked)
	    {
		    $textboxInputScript.Enabled = $false
	        $buttonBrowseInput.Enabled = $false
	        $textboxScriptFunction.Enabled = $true
	        $checkboxExcludeFunctionSource.Checked = $false
	        $checkboxExcludeFunctionSource.Enabled = $false
		}
	}
	
	$radiobuttonUseFile_CheckedChanged={
		$textboxInputScript.Enabled = $true
	    $buttonBrowseInput.Enabled = $true
	    $textboxScriptFunction.Enabled = $false
	    $checkboxExcludeFunctionSource.Enabled = $true
	}
	$buttonBrowseFolder_Click={
		if($folderbrowserdialog1.ShowDialog() -eq 'OK')
		{
			$textboxProjectFolder.Text = $folderbrowserdialog1.SelectedPath
		}
	}
	
	$checkboxCreateProject_CheckedChanged={
		if ($checkboxCreateProject.Checked)
	    {
	        $textboxProjectFolder.Enabled = $true
	        $buttonBrowseFolder.Enabled = $true
		}
	    else
	    {
	        $textboxProjectFolder.Enabled = $false
	        $buttonBrowseFolder.Enabled = $false
		}
	}
	
	$linklabelHttpwwwthelittlethin_LinkClicked=[System.Windows.Forms.LinkLabelLinkClickedEventHandler]{
	#Event Argument: $_ = [System.Windows.Forms.LinkLabelLinkClickedEventArgs]
		[Diagnostics.Process]::Start('http://www.the-little-things.net/','')
	}
	
	$checkboxLaunchAsSepProcess_CheckedChanged={
		if ($checkboxLaunchAsSepProcess.Checked)
	    {
	        $checkboxUseDatagridOutput.Checked = $false
	        $checkboxUseDatagridOutput.Enabled = $false
	        $checkboxExcludeFunctionSource.Checked = $false
	        $checkboxExcludeFunctionSource.Enabled = $false
		}
	    else
	    {
	        $checkboxUseDatagridOutput.Enabled = $true
	        $checkboxExcludeFunctionSource.Enabled = $true
		}
	}
	
	$checkboxUseDatagridOutput_CheckedChanged={
		if ($checkboxUseDatagridOutput.Checked)
	    {
	        $checkboxLaunchAsSepProcess.Checked = $false
	        $checkboxLaunchAsSepProcess.Enabled = $false
		}
	    else
	    {
	        $checkboxLaunchAsSepProcess.Enabled = $true
		}
	}
		# --End User Generated Script--
	#----------------------------------------------
	#region Generated Events
	#----------------------------------------------
	
	$Form_StateCorrection_Load=
	{
		#Correct the initial state of the form to prevent the .Net maximized form issue
		$CreateFormWrapper.WindowState = $InitialFormWindowState
	}
	
	$Form_StoreValues_Closing=
	{
		#Store the control values
		$script:MainForm_textboxScriptFunction = $textboxScriptFunction.Text
		$script:MainForm_radiobuttonUseText = $radiobuttonUseText.Checked
		$script:MainForm_radiobuttonUseFile = $radiobuttonUseFile.Checked
		$script:MainForm_textboxInputScript = $textboxInputScript.Text
		$script:MainForm_checkboxIncludeVerboseParame = $checkboxIncludeVerboseParame.Checked
		$script:MainForm_checkboxLaunchAsSepProcess = $checkboxLaunchAsSepProcess.Checked
		$script:MainForm_textboxProjectFolder = $textboxProjectFolder.Text
		$script:MainForm_checkboxCreateProject = $checkboxCreateProject.Checked
		$script:MainForm_checkboxExcludeFunctionSource = $checkboxExcludeFunctionSource.Checked
		$script:MainForm_checkboxUseDatagridOutput = $checkboxUseDatagridOutput.Checked
		$script:MainForm_checkbox_LaunchGUI = $checkbox_LaunchGUI.Checked
		$script:MainForm_textboxFile = $textboxFile.Text
	}

	
	$Form_Cleanup_FormClosed=
	{
		#Remove all event handlers from the controls
		try
		{
			$linklabelHttpwwwthelittlethin.remove_LinkClicked($linklabelHttpwwwthelittlethin_LinkClicked)
			$radiobuttonUseText.remove_CheckedChanged($radiobuttonUseText_CheckedChanged)
			$radiobuttonUseFile.remove_CheckedChanged($radiobuttonUseFile_CheckedChanged)
			$buttonBrowseInput.remove_Click($buttonBrowseInput_Click)
			$checkboxLaunchAsSepProcess.remove_CheckedChanged($checkboxLaunchAsSepProcess_CheckedChanged)
			$buttonBrowseFolder.remove_Click($buttonBrowseFolder_Click)
			$checkboxCreateProject.remove_CheckedChanged($checkboxCreateProject_CheckedChanged)
			$checkboxUseDatagridOutput.remove_CheckedChanged($checkboxUseDatagridOutput_CheckedChanged)
			$buttonBrowse.remove_Click($buttonBrowse_Click)
			$buttonGo.remove_Click($buttonGo_Click)
			$CreateFormWrapper.remove_Load($form1_FadeInLoad)
			$timerFadeIn.remove_Tick($timerFadeIn_Tick)
			$CreateFormWrapper.remove_Load($Form_StateCorrection_Load)
			$CreateFormWrapper.remove_Closing($Form_StoreValues_Closing)
			$CreateFormWrapper.remove_FormClosed($Form_Cleanup_FormClosed)
		}
		catch [Exception]
		{ }
	}
	#endregion Generated Events

	#----------------------------------------------
	#region Generated Form Code
	#----------------------------------------------
	#
	# CreateFormWrapper
	#
	$CreateFormWrapper.Controls.Add($labelVersion03betaAuthorZ)
	$CreateFormWrapper.Controls.Add($linklabelHttpwwwthelittlethin)
	$CreateFormWrapper.Controls.Add($groupboxScriptInput)
	$CreateFormWrapper.Controls.Add($groupboxScriptOutput)
	$CreateFormWrapper.Controls.Add($buttonGo)
	$CreateFormWrapper.AcceptButton = $buttonGo
	$CreateFormWrapper.ClientSize = '604, 475'
	$CreateFormWrapper.FormBorderStyle = 'FixedDialog'
	$CreateFormWrapper.MaximizeBox = $False
	$CreateFormWrapper.MinimizeBox = $False
	$CreateFormWrapper.Name = "CreateFormWrapper"
	$CreateFormWrapper.StartPosition = 'CenterScreen'
	$CreateFormWrapper.Text = "Create Powershell GUI Form Wrapper"
	$CreateFormWrapper.add_Load($form1_FadeInLoad)
	#
	# labelVersion03betaAuthorZ
	#
	$labelVersion03betaAuthorZ.Anchor = 'Bottom, Left'
	$labelVersion03betaAuthorZ.Location = '12, 426'
	$labelVersion03betaAuthorZ.Name = "labelVersion03betaAuthorZ"
	$labelVersion03betaAuthorZ.Size = '181, 34'
	$labelVersion03betaAuthorZ.TabIndex = 10
	$labelVersion03betaAuthorZ.Text = "Version: 0.3 (beta)
Author: Zachary Loeber"
	#
	# linklabelHttpwwwthelittlethin
	#
	$linklabelHttpwwwthelittlethin.Anchor = 'Bottom'
	$linklabelHttpwwwthelittlethin.Location = '211, 437'
	$linklabelHttpwwwthelittlethin.Name = "linklabelHttpwwwthelittlethin"
	$linklabelHttpwwwthelittlethin.Size = '280, 23'
	$linklabelHttpwwwthelittlethin.TabIndex = 13
	$linklabelHttpwwwthelittlethin.TabStop = $True
	$linklabelHttpwwwthelittlethin.Text = "http://www.the-little-things.net/"
	$linklabelHttpwwwthelittlethin.add_LinkClicked($linklabelHttpwwwthelittlethin_LinkClicked)
	#
	# groupboxScriptInput
	#
	$groupboxScriptInput.Controls.Add($textboxScriptFunction)
	$groupboxScriptInput.Controls.Add($radiobuttonUseText)
	$groupboxScriptInput.Controls.Add($radiobuttonUseFile)
	$groupboxScriptInput.Controls.Add($textboxInputScript)
	$groupboxScriptInput.Controls.Add($buttonBrowseInput)
	$groupboxScriptInput.Anchor = 'Top, Bottom, Left, Right'
	$groupboxScriptInput.Location = '13, 13'
	$groupboxScriptInput.Name = "groupboxScriptInput"
	$groupboxScriptInput.Size = '579, 254'
	$groupboxScriptInput.TabIndex = 8
	$groupboxScriptInput.TabStop = $False
	$groupboxScriptInput.Text = "Script Input"
	#
	# textboxScriptFunction
	#
	$textboxScriptFunction.Anchor = 'Top, Bottom, Left, Right'
	$textboxScriptFunction.Enabled = $False
	$textboxScriptFunction.Location = '6, 79'
	$textboxScriptFunction.MaxLength = 65534
	$textboxScriptFunction.Multiline = $True
	$textboxScriptFunction.Name = "textboxScriptFunction"
	$textboxScriptFunction.ScrollBars = 'Both'
	$textboxScriptFunction.Size = '563, 169'
	$textboxScriptFunction.TabIndex = 4
	#
	# radiobuttonUseText
	#
	$radiobuttonUseText.Location = '7, 49'
	$radiobuttonUseText.Name = "radiobuttonUseText"
	$radiobuttonUseText.Size = '114, 24'
	$radiobuttonUseText.TabIndex = 3
	$radiobuttonUseText.Text = "Use Text"
	$radiobuttonUseText.UseVisualStyleBackColor = $True
	$radiobuttonUseText.add_CheckedChanged($radiobuttonUseText_CheckedChanged)
	#
	# radiobuttonUseFile
	#
	$radiobuttonUseFile.Checked = $True
	$radiobuttonUseFile.Location = '7, 19'
	$radiobuttonUseFile.Name = "radiobuttonUseFile"
	$radiobuttonUseFile.Size = '104, 24'
	$radiobuttonUseFile.TabIndex = 0
	$radiobuttonUseFile.TabStop = $True
	$radiobuttonUseFile.Text = "Use File"
	$radiobuttonUseFile.UseVisualStyleBackColor = $True
	$radiobuttonUseFile.add_CheckedChanged($radiobuttonUseFile_CheckedChanged)
	#
	# textboxInputScript
	#
	$textboxInputScript.Anchor = 'Top, Left, Right'
	$textboxInputScript.AutoCompleteMode = 'SuggestAppend'
	$textboxInputScript.AutoCompleteSource = 'FileSystem'
	$textboxInputScript.Location = '117, 21'
	$textboxInputScript.Name = "textboxInputScript"
	$textboxInputScript.Size = '417, 20'
	$textboxInputScript.TabIndex = 1
	#
	# buttonBrowseInput
	#
	$buttonBrowseInput.Anchor = 'Top, Right'
	$buttonBrowseInput.Location = '540, 19'
	$buttonBrowseInput.Name = "buttonBrowseInput"
	$buttonBrowseInput.Size = '29, 23'
	$buttonBrowseInput.TabIndex = 2
	$buttonBrowseInput.Text = "..."
	$buttonBrowseInput.UseVisualStyleBackColor = $True
	$buttonBrowseInput.add_Click($buttonBrowseInput_Click)
	#
	# groupboxScriptOutput
	#
	$groupboxScriptOutput.Controls.Add($checkboxIncludeVerboseParame)
	$groupboxScriptOutput.Controls.Add($checkboxLaunchAsSepProcess)
	$groupboxScriptOutput.Controls.Add($buttonBrowseFolder)
	$groupboxScriptOutput.Controls.Add($textboxProjectFolder)
	$groupboxScriptOutput.Controls.Add($checkboxCreateProject)
	$groupboxScriptOutput.Controls.Add($checkboxExcludeFunctionSource)
	$groupboxScriptOutput.Controls.Add($checkboxUseDatagridOutput)
	$groupboxScriptOutput.Controls.Add($checkbox_LaunchGUI)
	$groupboxScriptOutput.Controls.Add($textboxFile)
	$groupboxScriptOutput.Controls.Add($labelSaveAs)
	$groupboxScriptOutput.Controls.Add($buttonBrowse)
	$groupboxScriptOutput.Anchor = 'Bottom, Left, Right'
	$groupboxScriptOutput.Location = '12, 273'
	$groupboxScriptOutput.Name = "groupboxScriptOutput"
	$groupboxScriptOutput.Size = '579, 140'
	$groupboxScriptOutput.TabIndex = 7
	$groupboxScriptOutput.TabStop = $False
	$groupboxScriptOutput.Text = "Script Output"
	#
	# checkboxIncludeVerboseParame
	#
	$checkboxIncludeVerboseParame.Location = '400, 14'
	$checkboxIncludeVerboseParame.Name = "checkboxIncludeVerboseParame"
	$checkboxIncludeVerboseParame.Size = '171, 24'
	$checkboxIncludeVerboseParame.TabIndex = 14
	$checkboxIncludeVerboseParame.Text = "Include Verbose Parameter"
	$tooltip1.SetToolTip($checkboxIncludeVerboseParame, "If your script accepts the verbose switch then enable this option.")
	$checkboxIncludeVerboseParame.UseVisualStyleBackColor = $True
	#
	# checkboxLaunchAsSepProcess
	#
	$checkboxLaunchAsSepProcess.Location = '8, 44'
	$checkboxLaunchAsSepProcess.Name = "checkboxLaunchAsSepProcess"
	$checkboxLaunchAsSepProcess.Size = '187, 24'
	$checkboxLaunchAsSepProcess.TabIndex = 13
	$checkboxLaunchAsSepProcess.Text = "Launch in Seperate Process"
	$tooltip1.SetToolTip($checkboxLaunchAsSepProcess, "Causes the script to launch in a powershell console.")
	$checkboxLaunchAsSepProcess.UseVisualStyleBackColor = $True
	$checkboxLaunchAsSepProcess.add_CheckedChanged($checkboxLaunchAsSepProcess_CheckedChanged)
	#
	# buttonBrowseFolder
	#
	$buttonBrowseFolder.Anchor = 'Bottom, Right'
	$buttonBrowseFolder.Enabled = $False
	$buttonBrowseFolder.Location = '541, 102'
	$buttonBrowseFolder.Name = "buttonBrowseFolder"
	$buttonBrowseFolder.Size = '30, 23'
	$buttonBrowseFolder.TabIndex = 12
	$buttonBrowseFolder.Text = "..."
	$buttonBrowseFolder.UseVisualStyleBackColor = $True
	$buttonBrowseFolder.add_Click($buttonBrowseFolder_Click)
	#
	# textboxProjectFolder
	#
	$textboxProjectFolder.Anchor = 'Bottom, Left, Right'
	$textboxProjectFolder.AutoCompleteMode = 'SuggestAppend'
	$textboxProjectFolder.AutoCompleteSource = 'FileSystemDirectories'
	$textboxProjectFolder.Enabled = $False
	$textboxProjectFolder.Location = '153, 104'
	$textboxProjectFolder.Name = "textboxProjectFolder"
	$textboxProjectFolder.Size = '382, 20'
	$textboxProjectFolder.TabIndex = 11
	#
	# checkboxCreateProject
	#
	$checkboxCreateProject.Anchor = 'Bottom, Left'
	$checkboxCreateProject.Location = '8, 102'
	$checkboxCreateProject.Name = "checkboxCreateProject"
	$checkboxCreateProject.Size = '173, 24'
	$checkboxCreateProject.TabIndex = 10
	$checkboxCreateProject.Text = "Create Sapien Project"
	$checkboxCreateProject.UseVisualStyleBackColor = $True
	$checkboxCreateProject.add_CheckedChanged($checkboxCreateProject_CheckedChanged)
	#
	# checkboxExcludeFunctionSource
	#
	$checkboxExcludeFunctionSource.Location = '8, 14'
	$checkboxExcludeFunctionSource.Name = "checkboxExcludeFunctionSource"
	$checkboxExcludeFunctionSource.Size = '187, 24'
	$checkboxExcludeFunctionSource.TabIndex = 5
	$checkboxExcludeFunctionSource.Text = "Do Not Insert Function"
	$tooltip1.SetToolTip($checkboxExcludeFunctionSource, "Attempts to call the ps1 file directly instead of inserting it into the GUI")
	$checkboxExcludeFunctionSource.UseVisualStyleBackColor = $True
	#
	# checkboxUseDatagridOutput
	#
	$checkboxUseDatagridOutput.Location = '209, 44'
	$checkboxUseDatagridOutput.Name = "checkboxUseDatagridOutput"
	$checkboxUseDatagridOutput.Size = '164, 24'
	$checkboxUseDatagridOutput.TabIndex = 7
	$checkboxUseDatagridOutput.Text = "Use Datagrid Output Form"
	$tooltip1.SetToolTip($checkboxUseDatagridOutput, "If the source script output is an array of psbojects select this option to automatically show the results in a gridview (with the option of exporting to a CSV)")
	$checkboxUseDatagridOutput.UseVisualStyleBackColor = $True
	$checkboxUseDatagridOutput.add_CheckedChanged($checkboxUseDatagridOutput_CheckedChanged)
	#
	# checkbox_LaunchGUI
	#
	$checkbox_LaunchGUI.Location = '209, 14'
	$checkbox_LaunchGUI.Name = "checkbox_LaunchGUI"
	$checkbox_LaunchGUI.Size = '164, 24'
	$checkbox_LaunchGUI.TabIndex = 6
	$checkbox_LaunchGUI.Text = "Immediately Launch GUI"
	$tooltip1.SetToolTip($checkbox_LaunchGUI, "Launch GUI script after it has been created")
	$checkbox_LaunchGUI.UseVisualStyleBackColor = $True
	#
	# textboxFile
	#
	$textboxFile.Anchor = 'Bottom, Right'
	$textboxFile.AutoCompleteMode = 'SuggestAppend'
	$textboxFile.AutoCompleteSource = 'FileSystem'
	$textboxFile.Location = '98, 74'
	$textboxFile.Name = "textboxFile"
	$textboxFile.Size = '437, 20'
	$textboxFile.TabIndex = 8
	#
	# labelSaveAs
	#
	$labelSaveAs.Anchor = 'Bottom, Left'
	$labelSaveAs.Font = "Microsoft Sans Serif, 9.75pt, style=Bold"
	$labelSaveAs.Location = '8, 74'
	$labelSaveAs.Name = "labelSaveAs"
	$labelSaveAs.Size = '84, 20'
	$labelSaveAs.TabIndex = 2
	$labelSaveAs.Text = "Save As:"
	$labelSaveAs.TextAlign = 'MiddleRight'
	#
	# buttonBrowse
	#
	$buttonBrowse.Anchor = 'Bottom, Right'
	$buttonBrowse.Location = '541, 73'
	$buttonBrowse.Name = "buttonBrowse"
	$buttonBrowse.Size = '29, 23'
	$buttonBrowse.TabIndex = 9
	$buttonBrowse.Text = "..."
	$buttonBrowse.UseVisualStyleBackColor = $True
	$buttonBrowse.add_Click($buttonBrowse_Click)
	#
	# buttonGo
	#
	$buttonGo.Anchor = 'Bottom, Right'
	$buttonGo.Location = '516, 432'
	$buttonGo.Name = "buttonGo"
	$buttonGo.Size = '75, 23'
	$buttonGo.TabIndex = 14
	$buttonGo.Text = "Go!"
	$buttonGo.UseVisualStyleBackColor = $True
	$buttonGo.add_Click($buttonGo_Click)
	#
	# openfiledialog1
	#
	$openfiledialog1.CheckFileExists = $False
	$openfiledialog1.DefaultExt = "ps1"
	$openfiledialog1.Filter = "Powershell Script (.ps1)|*.ps1|All Files|*.*"
	$openfiledialog1.ShowHelp = $True
	#
	# timerFadeIn
	#
	$timerFadeIn.add_Tick($timerFadeIn_Tick)
	#
	# tooltip1
	#
	#
	# folderbrowserdialog1
	#
	#endregion Generated Form Code

	#----------------------------------------------

	#Save the initial state of the form
	$InitialFormWindowState = $CreateFormWrapper.WindowState
	#Init the OnLoad event to correct the initial state of the form
	$CreateFormWrapper.add_Load($Form_StateCorrection_Load)
	#Clean up the control events
	$CreateFormWrapper.add_FormClosed($Form_Cleanup_FormClosed)
	#Store the control values when form is closing
	$CreateFormWrapper.add_Closing($Form_StoreValues_Closing)
	#Show the Form
	return $CreateFormWrapper.ShowDialog()

}
#endregion Source: MainForm.pff

#Start the application
Main ($CommandLine)
