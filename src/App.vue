<template>
    <div class="unpack-api-ipa-info">
        <label for="file-input" :class="['file-input-button',{'disabled':disabled}]">选择文件</label>
        <input id="file-input" accept="" :disabled="disabled" type="file" @change="change">
        <p>{{read}}</p>
        <div class="res">
            <table  v-if="ext=='.apk'">
                <tr>
                    <th>fileName</th>
                    <td v-text="fileName"></td>
                </tr>
                <tr>
                    <th>fileSzie</th>
                    <td v-text="size"></td>
                </tr>
                <tr>
                    <th>Suffix</th>
                    <td v-text="ext"></td>
                </tr>
                <tr>
                    <th>versionCode</th>
                    <td v-text="infos.versionCode"></td>
                </tr>
                <tr>
                    <th>versionName</th>
                    <td v-text="infos.versionName"></td>
                </tr>
                <tr>
                    <th>compileSdkVersion</th>
                    <td v-text="infos.compileSdkVersion"></td>
                </tr>
                <tr>
                    <th>compileSdkVersionCodename</th>
                    <td v-text="infos.compileSdkVersionCodename"></td>
                </tr>
                <tr>
                    <th>package</th>
                    <td v-text="infos.package"></td>
                </tr>
                 <tr>
                    <th>platformBuildVersionCode</th>
                    <td v-text="infos.platformBuildVersionCode"></td>
                </tr>
                <tr>
                    <th>platformBuildVersionName</th>
                    <td v-text="infos.platformBuildVersionName"></td>
                </tr>
                <tr>
                    <th>platformBuildVersionName</th>
                    <td v-text="infos.platformBuildVersionName"></td>
                </tr>
            </table>
            <table v-if="ext=='.ipa'">
                <tr>
                    <th>fileName</th>
                    <td v-text="fileName"></td>
                </tr>
                <tr>
                    <th>fileSzie</th>
                    <td v-text="size"></td>
                </tr>
                <tr>
                    <th>Suffix</th>
                    <td v-text="ext"></td>
                </tr>
                <tr>
                    <th>BuildMachineOSBuild</th>
                    <td v-text="infos.BuildMachineOSBuild"></td>
                </tr>
                <tr>
                    <th>CFBundleDisplayName</th>
                    <td v-text="infos.CFBundleDisplayName"></td>
                </tr>
                <tr>
                    <th>CFBundleIdentifier</th>
                    <td v-text="infos.CFBundleIdentifier"></td>
                </tr>
                <tr>
                    <th>CFBundleInfoDictionaryVersion</th>
                    <td v-text="infos.CFBundleInfoDictionaryVersion"></td>
                </tr>
                <tr>
                    <th>CFBundleShortVersionString</th>
                    <td v-text="infos.CFBundleShortVersionString"></td>
                </tr>
                 <tr>
                    <th>DTCompiler</th>
                    <td v-text="infos.DTCompiler"></td>
                </tr>
                <tr>
                    <th>DTPlatformBuild</th>
                    <td v-text="infos.DTPlatformBuild"></td>
                </tr>
                <tr>
                    <th>DTPlatformVersion</th>
                    <td v-text="infos.DTPlatformVersion"></td>
                </tr>
                <tr>
                    <th>DTXcode</th>
                    <td v-text="infos.DTXcode"></td>
                </tr>
                 <tr>
                    <th>DTXcodeBuild</th>
                    <td v-text="infos.DTXcodeBuild"></td>
                </tr>
                 <tr>
                    <th>MinimumOSVersion</th>
                    <td v-text="infos.MinimumOSVersion"></td>
                </tr>
            </table>
        </div>
    </div>
</template>
<script>
const AppInfoParser = require('app-info-parser')
export default {
    data(){
        return {
            infos:{},
            fileName:"",
            disabled:false,
            ext:"",
            size:0,
            read:"请选择要查看的ipa文件或apk文件"
        }
    },
    methods:{
        change(e){
            this.reset()
            this.read="信息解码中..."
            this.disabled=true
            const file=e.target.files[0]
            this.fileName=file.name
            this.size=(file.size/1024/1024).toFixed(2) +"MB"
            this.ext=this.getType(file.name)
            if(this.ext!=".apk"&&this.ext!=".ipa"){
                alert("请选择apk或ipa文件")
                this.disabled=false
                this.read="文件选择错误"
                e.target.value=""
                return 
            }
                const parser = new AppInfoParser(file)
                parser.parse().then(result => {
                    this.infos=result
                    this.read="信息读取完成"
                    this.disabled=false
                    console.log(result)
                }).catch(err => {
                    console.log('err ----> ', err)
                    this.disabled=false
                    this.read="信息读取失败"
                })
          
            e.target.value=""
        },
        getType(file){
            var filename=file;
            var index1=filename.lastIndexOf(".");
            var index2=filename.length;
            var type=filename.substring(index1,index2);
            return type;
        },
        reset(){
            this.infos=""
            this.fileName=""
            this.disabled=false
            this.ext=""
            this.size=0
            this.read="请选择要查看的ipa文件或apk文件"
        }
    }
}
</script>
<style>
    .file-input-button{
        display: block;
        background: #0087ff;
        padding: 20px 0;
        text-align: center;
        color: #fff;
        border-radius: 3px;
    }
    .disabled{
        background: #ccc;
    }
    #file-input{
        opacity: 0;
        display: none;
    }
    .unpack-api-ipa-info{
        margin: 0 auto;
        padding: 80px;
    }
    table{
        width: 100%;
    }
    table tr{
        padding: 15px;
    }
    table tr:hover{
        background: #eee;
    }
    table th{
        padding: 5px;
        text-align: right;
        border: 1px solid #eee;
        font-weight: normal;
        color: #444;
    }
    table td{
        width: 80%;
        text-indent: 2em;
        border: 1px solid #eee;
    }
</style>
