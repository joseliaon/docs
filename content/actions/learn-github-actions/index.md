  <!-- Copied from Microsoft.Common.CurrentVersion.targets -->
  <Target
      Name="_DeploymentSignClickOnceDeployment">

    <!-- Sign manifests and the bootstrapper -->
    <SignFile
        CertificateThumbprint="$(_DeploymentResolvedManifestCertificateThumbprint)"
        TimestampUrl="$(ManifestTimestampUrl)"
        SigningTarget="$(_DeploymentApplicationDir)$(_DeploymentTargetApplicationManifestFileName)"
        TargetFrameworkVersion="v4.0"
        Condition="'$(_DeploymentSignClickOnceManifests)'=='true'" />

    <!-- Update entry point path in deploy manifest -->
    <UpdateManifest
        ApplicationPath="$(_DeploymentApplicationFolderName)\$(_DeploymentTargetApplicationManifestFileName)"
        TargetFrameworkVersion="$(TargetFrameworkVersion)"
        ApplicationManifest="$(_DeploymentApplicationDir)$(_DeploymentTargetApplicationManifestFileName)"
        InputManifest="$(OutDir)$(TargetDeployManifestFileName)"
        OutputManifest="$(PublishDir)$(TargetDeployManifestFileName)">

      <Output TaskParameter="OutputManifest" ItemName="PublishedDeployManifest"/>

    </UpdateManifest>

    <SignFile
        CertificateThumbprint="$(_DeploymentResolvedManifestCertificateThumbprint)"
        TimestampUrl="$(ManifestTimestampUrl)"
        SigningTarget="$(PublishDir)$(TargetDeployManifestFileName)"
        TargetFrameworkVersion="v4.0"
        Condition="'$(_DeploymentSignClickOnceManifests)'=='true'" />

    <SignFile
        CertificateThumbprint="$(_DeploymentResolvedManifestCertificateThumbprint)"
        TimestampUrl="$(ManifestTimestampUrl)"
        SigningTarget="$(PublishDir)\setup.exe"
        Condition="'$(BootstrapperEnabled)'=='true' and '$(_DeploymentSignClickOnceManifests)'=='true'" />


  </Target>
