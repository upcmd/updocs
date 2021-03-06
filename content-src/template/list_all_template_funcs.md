---
title: "all template funcs"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 903
---

### List of all template funcs

You can use CLI cmd to check all supported golang template funcs
 
```
Ξ ▶ up assist all_template_func
-assist: all_template_func
=List of all template funcs
                        printf : "fmt.Sprintf"
                            lt : "lt"
                           not : "not"
                            or : "or"
                      urlquery : "URLQueryEscaper"
                            ge : "ge"
                          html : "HTMLEscaper"
                           len : "length"
                         index : "index"
                            eq : "eq"
                            js : "JSEscaper"
                         print : "fmt.Sprint"
                       println : "fmt.Sprintln"
                            gt : "gt"
                            le : "le"
                            ne : "ne"
                           and : "and"
                          call : "call"
                         title : (func(string) string)(0x10f4440)
                           int : (func(interface {}) int)(0x17137a0)
                    mustAppend : (func(interface {}, interface {}) ([]interface {}, error))(0x170fdb0)
                      capfirst : (func(string) string)(0x1720340)
                          base : (func(string) string)(0x11afed0)
                           dir : (func(string) string)(0x11b0010)
                         yesno : (func(string, string, bool) string)(0x1720880)
                       nindent : (func(int, string) string)(0x1715ba0)
                      mustRest : (func(interface {}) ([]interface {}, error))(0x1710da0)
                        append : (func(interface {}, interface {}) []interface {})(0x170fd00)
                        typeIs : (func(string, interface {}) bool)(0x1713f70)
                        center : (func(int, string) string)(0x1720b60)
                 truncatechars : (func(int, string) string)(0x171f0d0)
                          push : (func(interface {}, interface {}) []interface {})(0x170fd00)
                   pathExisted : (func(string) bool)(0x174a9a0)
                           mul : (func(interface {}, ...interface {}) int64)(0x17183e0)
                     hasPrefix : (func(string, string) bool)(0x1717f10)
                        plural : (func(string, string, int) string)(0x1715cd0)
                 durationRound : (func(interface {}) string)(0x170d450)
                      urlParse : (func(string) map[string]interface {})(0x1717050)
                      intcomma : (func(interface {}) string)(0x1721730)
                        substr : (func(int, int, string) string)(0x1716c30)
                          trim : (func(string) string)(0x10f4c90)
                     deepEqual : (func(interface {}, interface {}) bool)(0x1086080)
                        semver : (func(string) (*semver.Version, error))(0x1714db0)
                htmlDateInZone : (func(interface {}, string) string)(0x170cd70)
                           cat : (func(...interface {}) string)(0x1715960)
              mustRegexFindAll : (func(string, string, int) ([]string, error))(0x17145b0)
                         first : (func(interface {}) interface {})(0x17109d0)
                     sortAlpha : (func(interface {}) []string)(0x1711540)
                     mustMerge : (func(map[string]interface {}, ...map[string]interface {}) (interface {}, error))(0x170f3f0)
                       float64 : (func(interface {}) float64)(0x1713740)
                          ceil : (func(interface {}) float64)(0x1713cc0)
                derivePassword : (func(uint32, string, string, string, string) string)(0x1709500)
                      mustUniq : (func(interface {}) ([]interface {}, error))(0x1712080)
                  regexFindAll : (func(string, string, int) []string)(0x1714510)
                         genCA : (func(string, int) (sprig.certificate, error))(0x170a9f0)
                         slice : (func(interface {}, ...interface {}) interface {})(0x1712de0)
                mergeOverwrite : (func(map[string]interface {}, ...map[string]interface {}) interface {})(0x170f520)
                    trimPrefix : (func(string, string) string)(0x1717cf0)
                         isAbs : (func(string) bool)(0x11afff0)
                        toDate : (func(string, string) time.Time)(0x170d8e0)
                         hello : (func() string)(0x1717a50)
                       trimall : (func(string, string) string)(0x1717b30)
                          add1 : (func(interface {}) int64)(0x1718110)
                       replace : (func(string, string, string) string)(0x1715c30)
                      mustLast : (func(interface {}) (interface {}, error))(0x1710720)
                     urlencode : (func(string) string)(0x171f300)
                         unset : (func(map[string]interface {}, string) map[string]interface {})(0x170e8e0)
                         trunc : (func(int, string) string)(0x1716730)
                       mustHas : (func(interface {}, interface {}) (bool, error))(0x1712ac0)
                           max : (func(interface {}, ...interface {}) int64)(0x1713840)
                    trimSuffix : (func(string, string) string)(0x1717c10)
                          ARCH : (func() string)(0x174a210)
                    mustToJson : (func(interface {}) (string, error))(0x170e1e0)
                     sha256sum : (func(string) string)(0x1709050)
                          omit : (func(map[string]interface {}, ...string) map[string]interface {})(0x170eea0)
                filesizeformat : (func(interface {}) string)(0x1720d00)
                         ljust : (func(int, string) string)(0x1720a60)
                           set : (func(map[string]interface {}, string, interface {}) map[string]interface {})(0x170e840)
                       urlJoin : (func(map[string]interface {}) string)(0x1717650)
                       default : (func(interface {}, ...interface {}) interface {})(0x170dad0)
                        b64dec : (func(string) string)(0x1714ec0)
                   mustPrepend : (func(interface {}, interface {}) ([]interface {}, error))(0x1710220)
                         quote : (func(...interface {}) string)(0x17154d0)
                 genPrivateKey : (func(string) string)(0x1709d40)
                       nospace : (func(string) string)(0x16fdfc0)
                        abbrev : (func(int, string) string)(0x1715100)
                         until : (func(int) []int)(0x17139e0)
                     unixEpoch : (func(time.Time) string)(0x170da40)
                    typeIsLike : (func(string, interface {}) bool)(0x1714030)
                     camelcase : (func(string) string)(0x16fee40)
                        uuidv4 : (func() string)(0x17093e0)
                          date : (func(string, interface {}) string)(0x170cc50)
                           add : (func(...interface {}) int64)(0x1718170)
                       ternary : (func(interface {}, interface {}, bool) interface {})(0x170e750)
                        b32enc : (func(string) string)(0x1714f90)
                mustDateModify : (func(string, time.Time) (time.Time, error))(0x170d1f0)
                           mod : (func(interface {}, interface {}) int64)(0x1718340)
                  toPrettyJson : (func(interface {}) string)(0x170e2a0)
                          rest : (func(interface {}) []interface {})(0x1710d10)
                    regexSplit : (func(string, string, int) []string)(0x1714b10)
                        hasKey : (func(map[string]interface {}, string) bool)(0x170e950)
                   fileContent : (func(string) string)(0x174aad0)
                         deReg : (func(string) string)(0x174a8b0)
                           now : (func() string)(0x174a430)
                   date_modify : (func(string, time.Time) time.Time)(0x170d120)
                     wordcount : (func(string) int)(0x171f3b0)
                  date_in_zone : (func(string, interface {}, string) string)(0x170ce00)
                       untitle : (func(string) string)(0x1715460)
                       initial : (func(interface {}) []interface {})(0x1711140)
                       shuffle : (func(string) string)(0x16ffb20)
                         int64 : (func(interface {}) int64)(0x17137f0)
                      printObj : (func(interface {}) string)(0x174a4a0)
                       biggest : (func(interface {}, ...interface {}) int64)(0x1713840)
                     pluralize : (func(string, interface {}) string)(0x1720460)
                     fromSlash : (func(string) string)(0x174a3f0)
                     toRawJson : (func(interface {}) string)(0x170e440)
                        kindOf : (func(interface {}) string)(0x17142c0)
           mustRegexReplaceAll : (func(string, string, string) (string, error))(0x1714890)
                      deepCopy : (func(interface {}) interface {})(0x170fa00)
                      toString : (func(interface {}) string)(0x17164f0)
                    decryptAES : (func(string, string) (string, error))(0x170c8d0)
                           sub : (func(interface {}, interface {}) int64)(0x1718220)
   validateMandatoryFailIfNone : (func(string, string) string)(0x174af20)
                      coalesce : (func(...interface {}) interface {})(0x170e090)
                        toJson : (func(interface {}) string)(0x170e150)
                        concat : (func(...interface {}) interface {})(0x17131e0)
                      objToYml : (func(interface {}) string)(0x174a570)
                     regexFind : (func(string, string) string)(0x17146a0)
                     toStrings : (func(interface {}) []string)(0x1715d10)
                         floor : (func(interface {}) float64)(0x1713c40)
                        b64enc : (func(string) string)(0x1714e20)
                   mustInitial : (func(interface {}) ([]interface {}, error))(0x17111d0)
                randomintrange : (func(int, int, interface {}) int)(0x1723120)
                       toSlash : (func(string) string)(0x174a410)
                        splitn : (func(string, int, string) map[string]string)(0x1716a50)
                     expandenv : (func(string) string)(0x1718510)
                          atoi : (func(string) int)(0x1718020)
                    regexMatch : (func(string, string) bool)(0x1714420)
                     toDecimal : (func(interface {}) int64)(0x1713eb0)
                      wrapWith : (func(int, string, string) string)(0x1717e20)
                         rjust : (func(int, string) string)(0x1720940)
               regexReplaceAll : (func(string, string, string) string)(0x17147f0)
                         round : (func(interface {}, int, ...float64) float64)(0x1713d40)
                     mustFirst : (func(interface {}) (interface {}, error))(0x1710a60)
                      apnumber : (func(interface {}) interface {})(0x1721220)
                     randAscii : (func(int) string)(0x1715370)
                       trimAll : (func(string, string) string)(0x1717ba0)
                           reg : (func(string, interface {}) string)(0x174a710)
                   mustWithout : (func(interface {}, ...interface {}) ([]interface {}, error))(0x1712610)
               buildCustomCert : (func(string, string) (sprig.certificate, error))(0x170a480)
                           ext : (func(string) string)(0x11afe80)
                      lengthis : (func(int, interface {}) bool)(0x17200c0)
                      catLines : (func(string) string)(0x174a230)
                    dateInZone : (func(string, interface {}, string) string)(0x170ce00)
        regexReplaceAllLiteral : (func(string, string, string) string)(0x1714980)
                           div : (func(interface {}, interface {}) int64)(0x17182a0)
                          wrap : (func(int, string) string)(0x1717da0)
                          uniq : (func(interface {}) []interface {})(0x1711ff0)
                    dateModify : (func(string, time.Time) time.Time)(0x170d120)
                        repeat : (func(int, string) string)(0x1717ac0)
                 semverCompare : (func(string, string) (bool, error))(0x1714ca0)
                         pluck : (func(string, ...map[string]interface {}) []interface {})(0x170e9c0)
                          pick : (func(map[string]interface {}, ...string) map[string]interface {})(0x170ed50)
                        values : (func(map[string]interface {}) []interface {})(0x170f870)
                    encryptAES : (func(string, string) (string, error))(0x170c440)
                           env : (func(string) string)(0x17184b0)
                       ordinal : (func(interface {}) string)(0x1721d40)
                      mustPush : (func(interface {}, interface {}) ([]interface {}, error))(0x170fdb0)
                          last : (func(interface {}) interface {})(0x1710690)
                       compact : (func(interface {}) []interface {})(0x1711b70)
                    abbrevboth : (func(int, int, string) string)(0x1715190)
                        length : (func(interface {}) int)(0x171eda0)
                       without : (func(interface {}, ...interface {}) []interface {})(0x1712550)
                     mustSlice : (func(interface {}, ...interface {}) (interface {}, error))(0x1712e90)
                    splitLines : (func(string) []string)(0x174a310)
                       reverse : (func(interface {}) []interface {})(0x1711790)
                      contains : (func(string, string) bool)(0x1717ea0)
                  randAlphaNum : (func(int) string)(0x17152b0)
                   randNumeric : (func(int) string)(0x17153f0)
                          fail : (func(string) (string, error))(0x1718580)
                     kebabcase : (func(string) string)(0x16ff240)
                   mustReverse : (func(interface {}) ([]interface {}, error))(0x1711820)
                        random : (func(interface {}) interface {})(0x1722cd0)
                          join : (func(string, interface {}) string)(0x17167d0)
                            OS : (func() string)(0x174a1f0)
                      htmlDate : (func(interface {}) string)(0x170cce0)
                    adler32sum : (func(string) string)(0x17092e0)
                        squote : (func(...interface {}) string)(0x1715730)
                           get : (func(map[string]interface {}, string) interface {})(0x170e790)
                 genSignedCert : (func(string, []interface {}, []interface {}, int, sprig.certificate) (sprig.certificate, error))(0x170ae90)
                        kindIs : (func(string, interface {}) bool)(0x1714240)
             genSelfSignedCert : (func(string, []interface {}, []interface {}, int) (sprig.certificate, error))(0x170ac30)
                           has : (func(interface {}, interface {}) bool)(0x1712a30)
                      swapcase : (func(string) string)(0x16fe830)
                          dict : (func(...interface {}) map[string]interface {})(0x170f130)
                         split : (func(string, string) map[string]string)(0x1716870)
                     splitList : (func(string, string) []string)(0x1718070)
                        typeOf : (func(interface {}) string)(0x17141a0)
              mustToPrettyJson : (func(interface {}) (string, error))(0x170e350)
                    mustToDate : (func(string, string) (time.Time, error))(0x170d980)
                   divisibleby : (func(interface {}, interface {}) bool)(0x171f450)
                        b32dec : (func(string) string)(0x1715030)
                          list : (func(...interface {}) []interface {})(0x170fce0)
                         upper : (func(string) string)(0x10f3d10)
                   mustCompact : (func(interface {}) ([]interface {}, error))(0x1711c00)
                   findreplace : (func(string, string, string) string)(0x171e9e0)
                 getHostByName : (func(string) string)(0x1713660)
                      ymlToObj : (func(string) interface {})(0x174a650)
    mustRegexReplaceAllLiteral : (func(string, string, string) (string, error))(0x1714a20)
                     untilStep : (func(int, int, int) []int)(0x1713a60)
              must_date_modify : (func(string, time.Time) (time.Time, error))(0x170d1f0)
                           min : (func(interface {}, ...interface {}) int64)(0x1713910)
                     snakecase : (func(string) string)(0x16ff1d0)
                         tuple : (func(...interface {}) []interface {})(0x170fce0)
                 mustToRawJson : (func(interface {}) (string, error))(0x170e4c0)
                  mustDeepCopy : (func(interface {}) (interface {}, error))(0x170faf0)
                       sha1sum : (func(string) string)(0x17091a0)
                         merge : (func(map[string]interface {}, ...map[string]interface {}) interface {})(0x170f2d0)
            mustMergeOverwrite : (func(map[string]interface {}, ...map[string]interface {}) (interface {}, error))(0x170f6c0)
                         clean : (func(string) string)(0x11af3b0)
                mustRegexMatch : (func(string, string) (bool, error))(0x1714490)
                         empty : (func(interface {}) bool)(0x170dbd0)
                mustRegexSplit : (func(string, string, int) ([]string, error))(0x1714bb0)
                      initials : (func(string) string)(0x1715240)
                     striptags : (func(string) string)(0x17232a0)
                          keys : (func(...map[string]interface {}) []string)(0x170eb70)
                        indent : (func(int, string) string)(0x1715a60)
                     randAlpha : (func(int) string)(0x1715300)
                       prepend : (func(interface {}, interface {}) []interface {})(0x1710170)
                           ago : (func(interface {}) string)(0x170d2d0)
                         lower : (func(string) string)(0x10f4000)
                     hasSuffix : (func(string, string) bool)(0x1717f80)
                 mustRegexFind : (func(string, string) (string, error))(0x1714720)
```
